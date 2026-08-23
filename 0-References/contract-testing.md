# 契约测试（Contract Testing）— 消费者驱动契约

> 引用来源：Pact Foundation 官方文档 https://docs.pact.io/ ；Martin Fowler, "ContractTest" https://martinfowler.com/bliki/ContractTest.html
> 在 SDIE 中引用位置：2-Design/test-strategy.template.md（集成层 CDC、门禁）、SDIE-Design-Guide.md §5 / §6.3
> 用途：为服务间（含 gRPC / 异步消息）接口提供「轻量、可并行、早失败」的兼容性验证，作为测试金字塔中集成层的安全网。

## 定义

契约测试验证**服务调用方（Consumer）**与**提供方（Provider）**对同一份「接口约定」的各自实现是否匹配，**不要求把双方真实环境连起来**。主流实践为**消费者驱动契约（Consumer-Driven Contracts, CDC）**：契约以消费方期望为准，由消费方编写并发布，提供方拉取后对自己的真实服务进行验证。

核心抽象：任何一次交互都可建模为「输入请求 / 调用 → 期望输出响应 / 错误」，契约测试关心的是**编解码后消息内容的结构与语义**，与底层传输协议（HTTP / gRPC / 消息队列）无关。

## 核心要素

- **契约文件（Contract / Pact file）**：描述一组 interaction（请求 + 期望响应）的版本化文件，独立于双方代码仓库，存于**契约仓库**（Pact Broker / 共享目录）。
- **Consumer 侧**：按自身调用方式编写 expectation，本地用由契约生成的 mock 跑本方逻辑；同时把 expectation **发布为契约文件**。
- **Provider 侧**：拉取契约，**对真实服务**发起请求，验证返回满足契约声明的响应结构（字段、类型、必选/可选、值域）。
- **匹配器（Matcher）**：如 `like`、`eachLike`、正则、类型匹配——契约声明「结构规则」而非「固定值」，避免脆弱断言。
- **契约仓库 / Broker**：集中存储契约、记录 Provider 验证结果、支持「我改了接口会影响哪些消费方」的可见性（can-i-deploy 校验）。

## 适用协议（SDIE 采用矩阵）

| 形态 | 支持度 | SDIE 约定 |
|------|:-------:|-----------|
| HTTP / REST | ⭐⭐⭐ 原生支持 | 常规用例，匹配器直接作用于 JSON |
| gRPC | ⭐⭐⭐ 成熟 | 用 Pact + `pact-protobuf-plugin`；`proto` 文件即强类型契约，支持 unary / streaming 四种模式 |
| 异步消息（Kafka / RabbitMQ / MQ） | ⭐⭐⭐ 成熟 | 用 Pact **Message Pact**，验证消息体结构（即「异步 RPC」契约） |
| Thrift / Dubbo | ⭐⭐ 可用 | IDL / Java 接口即契约；Pact 无官方适配，需自写 plugin 或泛化调用验证，方案须记 ADR |

## 使用约定（SDIE 落地规则）

### 约定 1：何时需要契约测试
- 跨团队 / 跨模块、接口**稳定且被多方依赖**的服务间调用，必须在「集成层」引入 CDC 契约测试。
- 单进程内、或一次性 / 无外部依赖的接口**不**需要（用单元测试覆盖即可）。
- 在 test-strategy.template.md 的「契约测试适用清单」中显式列出涉及的服务对与协议。

### 约定 2：契约仓库与位置
- 契约文件统一存于专属契约仓库（Pact Broker 或 `docs/contracts/` 共享目录），**不散落在双方代码库**。
- 契约须版本化，破坏性变更遵循 SemVer（MAJOR = 契约级破坏性变更），与 SDIE-Design-Guide.md §7.3 版本策略联动。

### 约定 3：CDC 顺序不可颠倒
- 消费方**先**写 expectation 并发布契约；提供方**后**拉取验证。禁止提供方单方定义契约、消费方被动接受。
- 新增 / 修改接口：先改契约 → 双方确认（消费方 + 提供方 + Tech Lead 明知）→ 再各自改实现。

### 约定 4：Provider 侧必须打真实服务
- Provider 验证**不得**用 mock 替代真实服务；必须启动真实服务（真实 gRPC server / 真实消息消费者）接收契约请求。
- Consumer 侧才用由契约生成的 mock；双方不可同时 mock（否则契约形同虚设）。

### 约定 5：门禁门
- **Provider 的 PR 必须通过契约验证**才能合并（写进 Harness / Jenkinsfile）。
- 契约文件变更本身需经 Tech Lead 审视（联动不可委托 ② 架构选型 / 接口契约定稿 human-only）。
- 门禁阈值区块增加一项：`契约验证(contract): 通过（Provider CI 强制）`。

### 约定 6：契约 ≠ 文档 ≠ mock 测试
- 契约是**从测试代码生成、双方 CI 强制验证**的活物，不可退化为手工维护的接口文档。
- 契约测试**不能替代**集成 / E2E：只验证接口形状，不验证真实网络、鉴权、超时、序列化版本兼容等运行时行为。本项目将其归入「集成层」占比内（金字塔 集成 20%），非独立条数。

### 约定 7：匹配规则显式声明
- `proto` / IDL 是骨架；真正的契约还含匹配规则（字段可空性、值域、列表长度）与异常路径（错误码 / 重试语义），必须由测试代码显式声明，禁止用「全量固定值」硬断言。

### 约定 8：与 RACI 联动
- **QA（C）**：制定契约测试策略、适用清单、门禁草案（与 Tech Lead 共定，不可委托 ③）。
- **Dev（R）**：在各自服务实现 Consumer / Provider 侧契约用例。
- **Tech Lead（A，Gate 2）**：确认契约范围与接口契约定稿（②），契约级破坏性变更由其审批。
- **Reviewer（I）**：Review 时对齐「分层与契约」。

## 权威出处
- Pact Foundation. "Pact — A Contract Testing Tool." https://docs.pact.io/
- Fowler, M. "ContractTest." https://martinfowler.com/bliki/ContractTest.html
