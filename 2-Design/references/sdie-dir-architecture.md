---
id: DOC-SYS-DIR-001
title: 全栈目录结构总览（纯 DDD 内聚 · Polyrepo 垂直切片）
status: draft
phase: Design
owner: Tech Lead / 架构负责人
related_docs:
  - microservices-scaffolding-polyrepo.md
  - vue3-dir-architecture.md
  - x-cola5-architecture.md
  - x-cola5-light-architecture.md
last_updated: 2026-08-27
---

# 全栈目录结构总览（纯 DDD 内聚）

> 本文件**综合** `raw/design/` 下四份设计文档，给出一套端到端、可落地的目录结构蓝图：
> - **骨架**来自 `microservices-scaffolding-polyrepo.md`（范式 C·垂直切片：多 git 仓，每域一仓且前后端同仓）；
> - **前端内部**来自 `vue3-dir-architecture.md`（方案 A·纯 DDD 内聚的 `modules/<feature>/`）；
> - **后端服务内部**来自 `x-cola5-architecture.md` / `x-cola5-light-architecture.md`（COLA 5 分层 + 按领域聚合组织）。
>
> **贯穿原则：纯 DDD 内聚**——从组织级到代码对象级，逐层用"限界上下文 / 业务域 / 特性 / 聚合"切分，拒绝按技术职能横切。
>
> **占位符约定**：通用规则一律用占位符，具体业务名仅作为中文语义提示出现在注释中。占位符清单：`<org>`（组织根）、`<domain>`（业务域仓）、`<web>`（域内前端应用）、`<svc>`（域内后端服务）、`<feature>`（前端模块）、`<aggregate>`（领域聚合）、`<Resource>`（契约/对象资源）、`<biz>`（应用层业务包）、`<train>`（发布列车）、`<version>`（版本号）。

---

## 0. DDD 内聚四级映射

| 层级 | 切分维度 | 结构落点 | 来源 |
|------|----------|----------|------|
| 组织级 | 限界上下文（Bounded Context） | 每个 git 仓 = 一个业务域 `<domain>` | polyrepo §2 |
| 域仓级 | 垂直切片 | 仓内 `apps/`(前端) + `services/`(后端) + `docs/`(SDIE) | polyrepo §3 |
| 前端模块级 | 业务特性 / 子域 | `apps/<web>/src/modules/<feature>/` 自包含 | vue3 §1 |
| 后端服务级 | 领域聚合（Aggregate） | `services/<svc>/.../domain/<aggregate>/` | cola §5 |
| 后端对象级 | 领域模型 | Entity / ValueObject / DomainEvent | cola §5 / §8 |

> 不变项（全部沿用 polyrepo）：frontmatter 七字段、`status` 状态机、RACI（A 属人）、Gate 1–4、不可委托 ①–⑩、Git 分支/标签约定。

---

## 1. 组织级多仓布局（org 级别）

```
<org>/                                          # 组织根（多 git 仓）
├── meta-sdie/                                  # ★ 治理仓（唯一跨域 SDIE 纪律载体）
│   ├── README.md                               # 多仓协作约定入口
│   ├── AGENTS.md                               # 全局 Agent 边界 / RACI 摘要 / 不可委托清单
│   ├── RACI.md                                 # 本项目 RACI 摘要（引自 SDIE-RACI-Matrix）
│   ├── .sdie/
│   │   ├── config.yaml                         # 全局 Gate 1-4 阈值 / 状态机 / SemVer 策略
│   │   ├── templates/                          # 四阶段模板权威副本（单一真源）
│   │   └── scripts/                            # frontmatter/status/gate 校验脚本（各域 CI 复用）
│   │       └── sync-templates.sh               # 将「钉选版本」模板推送到各 <domain>*/.sdie/templates 并开 PR
│   ├── docs/
│   │   ├── adr/                                # 跨域全局 ADR（红线②）
│   │   └── INDEX.md                            # ★ 全量域仓 + 服务/前端 + ADR 索引（跨仓追溯入口）
│   ├── packages/
│   │   └── contracts/                          # ★ 跨域契约注册（OpenAPI/Protobuf 单一真源）
│   └── releases/                               # ★ 发布列车（系统级迭代编排）
│       ├── train-v1.0.0/
│       │   ├── MANIFEST.md                     # 各域仓→版本映射
│       │   ├── gate-1.md … gate-4.md           # 系统级四道门禁核查
│       │   └── RELEASE-NOTES.md
│       └── train-v1.1.0/ ...
│
└── <domain>/                                   # ★ 业务域仓（示例：结算域 / 商品域 / 身份域 / 支付域，每域一仓）
```

> 每个 `<domain>` 是**独立的 git 仓库**，内部结构见 §2。

---

## 2. 单域仓内部结构（<domain> 示例：前端+后端同仓）

```
<domain>/                                        # 独立 git 仓（一个业务域，示例：结算域）
├── README.md                                   # 域说明：职责边界 / 含哪些服务与前端 / 依赖契约
├── AGENTS.md                                    # 继承 meta 全局约定，细化本域 Agent 边界与 RACI
├── .sdie/                                      # 薄治理层：模板「受控副本」（物理复制，非 meta 子仓引用）
│   └── templates/                              # 四阶段模板受控副本：由 meta-sdie 推送钉选版本，见 §7
│       └── SYNCED-FROM                         # 元数据：meta-sdie@<commit/version>，域 CI 校验漂移告警
├── docs/                                       # ★ 本域 SDIE AI Coding 制品（四阶段）
│   ├── 1-Spec/      (_templates/ PRD/ story-map/ acceptance_criteria/ task-specs/)
│   ├── 2-Design/    (decomposition/ test-strategy/ context-injection/ security-design/ adr/)
│   ├── 3-Implement/ (behavior-checklist/ case-delta/ review-checklist/ gate3-checklist/)
│   ├── 4-Evaluation/ (quality-dashboard/ release-decision/ adversarial-report/ business-value/ eval-metrics/ retrospective/)
│   └── iterations/  (iter-001/ : ITER-PLAN / MANIFEST / gates/ / RELEASE-NOTES / RETRO)
│
├── apps/                                       # ★ 本域前端应用（与后端同仓）
│   └── <web>/                                  # 本域前端应用（示例：结算域 Web 前端，Vue 3 + TS）→ 结构见 §3.1
├── services/                                   # ★ 本域后端微服务（一域可含多服务）
│   ├── <svc>/                                  # 本域后端 API 服务（示例：结算 API）→ 结构见 §3.2
│   └── <svc>/                                  # 本域后端异步 Worker（示例：结算 Worker，第二个服务）
├── e2e/features/                               # 本域跨服务验收测试（Gherkin 映射 acceptance_criteria）
├── infra/                                      # 本域部署（docker-compose / k8s / terraform）
├── scripts/  .github/ (或 ci/)  .workbuddy/
```

> **关键点**：前端 `apps/<web>/` 与后端 `services/<svc>/` 同在 `<domain>/` 仓内（满足"前后端同一仓库"）；跨域调用一律经 `meta-sdie/packages/contracts/` 契约，不直连他域代码。

---

## 3.1 前端应用结构（apps/<web>/）

> 采用 vue3-dir-architecture.md 的**方案 A·纯 DDD 内聚**：业务代码全部内聚到 `modules/<feature>/`，顶层仅保留聚合器与基础设施。

```
<web>/
├── envConfig/                 # 环境变量目录（vite envDir 指向此）
│   ├── .env  .env.development  .env.test  .env.production  .env.local.example
├── docs/                   # 项目文档（四阶段制品 docs/1-Spec..4-Evaluation 等）
├── public/                 # 固定 URL 静态资源（favicon/robots，不进打包）
├── mock/                   # 本地 mock 数据（开发期接口模拟，可选）
├── src/
│   ├── main.ts  App.vue
│   ├── modules/               # 业务模块（DDD 内聚单元）
│   │   ├── <feature>/         # 示例：<feature-a> / <feature-b> / <feature-c>（业务特性/子域）
│   │   │   ├── views/         # 页面级组件（路由挂载点）
│   │   │   ├── components/    # 模块私有组件
│   │   │   ├── composables/   # 模块逻辑组合（useXxx.ts）
│   │   │   ├── assets/        # 模块私有静态资源（images/fonts/data）
│   │   │   ├── services/      # 模块 API 服务（调用 src/api/http.ts）
│   │   │   ├── stores/        # 模块 Pinia store（useXxxStore.ts）
│   │   │   ├── types/         # 模块 TS 类型（index.ts 出口）
│   │   │   ├── router.ts      # 模块路由定义（导出 RouteRecordRaw[]）
│   │   │   └── index.ts       # 模块 Barrel 出口（仅 re-export 代码/类型，不含资源）
│   ├── shared/                # 跨模块共享代码（components/composables/utils/directives/plugins）
│   ├── layouts/               # 全局布局组件（非模块内）
│   ├── router/index.ts        # 主路由：聚合各模块 router.ts（懒加载）
│   ├── stores/index.ts        # 主 store：聚合导出各模块 store
│   ├── types/                 # 全局共享类型（跨模块复用）
│   ├── constants/             # 全局常量（枚举、固定配置值，编译期确定）
│   ├── config/                # 运行时配置（区别于 env，可被代码安全引用）
│   ├── locales/               # 国际化资源（i18n，可选）
│   ├── assets/                # 全局静态资源（images/styles/fonts）
│   ├── api/http.ts            # 全局 HTTP 客户端（axios 实例 + 拦截器）
│   └── env.d.ts
├── tests/  index.html  package.json  tsconfig.json  tsconfig.node.json
├── vite.config.ts  vitest.config.ts  .eslintrc.cjs  .prettierrc.cjs  .gitignore  README.md
```

**前端关键约束**（详见 vue3-dir-architecture.md）：
- 模块间**禁止**直接 import 对方内部；跨模块交互通过 `shared/types` 或各自 store 解耦（边界规则）。
- 静态资源二分：`src/`（含 `modules/<feature>/assets/`）走 Vite 哈希/内联；`public/` 仅放固定 URL 资源（favicon/robots）。
- 路径别名 `@/` 指向 `src/`；模块内部优先相对路径。

---

## 3.2 后端服务结构（services/<svc>/）

> 采用 COLA 5 **Light**（单 Maven 模块 + 包级分层）+ 独立 `client` 契约包，贴合 polyrepo 中 `services/<svc>/` 单 `src/` 的约定；需要编译期强制分层隔离时改用 COLA 5 完整多模块（见下方备选树）。

```
<svc>/                                          # 一个后端微服务（独立部署单元 / 一个业务子域的实现）
├── pom.xml                                    # 聚合父 pom：<modules> client, core
├── <svc>-client/                              # ★ 对外契约包（COLA client 模块，可独立 deploy jar）
│   └── src/main/java/{base}/client/
│       ├── api/                               # 接口（含 @RequestMapping 等 HTTP 注解）
│       └── dto/                               # 请求/响应/事件 DTO
├── <svc>-core/                                # ★ 实现模块（COLA Light：单模块 + 包级分层）
│   └── src/main/java/{base}/
│       ├── adapter/                           # 适配层：controller / api(http,rpc) / scheduler / listener / config
│       ├── application/                       # 应用层：service / executor(command,query) / eventhandler / processor / command / query / vo
│       ├── domain/                            # 领域层（按聚合组织）：<aggregate>/entity|service|gateway|event
│       └── infrastructure/                    # 基础设施层：<aggregate>/gateway/impl|mapper|dataobject ; common/client|event|config|util
├── Dockerfile  service.yaml  README.md
└── (各模块 src/test 放测试)
```

**依赖方向（编译期即强制）**：
```
adapter → application → domain ← infrastructure
adapter → client ; application → client ; infrastructure → client
```
- `domain` / `client` 不依赖任何业务模块；`adapter` 绝不依赖 `infrastructure`；消费方仅依赖 `client` jar。
- 领域层按**聚合（Aggregate）**组织，不按技术职能平铺；对象隔离：Cmd/Qry/VO 在 app，DTO 在 client，DO 在 infrastructure，Entity/V/Event 在 domain，互不越层泄漏。

**备选：COLA 5 完整多模块（需编译期强制分层隔离时）**

将 `adapter / app / domain / infrastructure` 各拆为独立 Maven 模块，并额外引入 `start` 启动模块（`-client` 契约模块二者皆有）。各模块独立 `pom.xml`，依赖方向在**编译期**即强制，从根本上杜绝 Light 版包级分层下可能出现的越层引用（如 adapter 误依赖 infrastructure）。

```
<svc>/                                          # 一个后端微服务（独立部署单元 / 一个业务子域的实现）
├── pom.xml                                    # 聚合父 pom：<modules> client, adapter, app, domain, infrastructure, start
├── <svc>-client/                              # ★ 对外契约模块（可独立 deploy jar，不依赖任何业务模块）
│   └── src/main/java/{base}/client/
│       ├── api/                               # 接口（含路径注解）
│       └── dto/                               # 请求/响应/事件 DTO
├── <svc>-adapter/                             # 适配层模块（按协议组织，不按领域）
│   └── src/main/java/{base}/adapter/
│       ├── controller/                        # Web REST（面向前端）
│       ├── api/                               # http(Feign) / rpc(Dubbo) Provider
│       ├── scheduler/                         # 定时任务
│       ├── listener/                          # MQ Consumer
│       └── config/
├── <svc>-app/                                 # 应用层模块（用例编排，按聚合组织）
│   └── src/main/java/{base}/app/<biz>/
│       ├── service/  executor/  eventhandler/  processor/  command/  query/  vo/   # 四层内包结构与 Light 版一致
├── <svc>-domain/                              # 领域层模块（按聚合组织，不依赖任何业务模块）
│   └── src/main/java/{base}/domain/
│       ├── <aggregate>/                        # 领域聚合目录（按聚合组织，同 Light 版 domain 层）
│       └── shared/event/
├── <svc>-infrastructure/                      # 基础设施层模块（实现 domain Gateway）
│   └── src/main/java/{base}/infrastructure/
│       ├── <aggregate>/                        # 聚合实现目录（gateway/impl|mapper|dataobject，同 Light 版 infra 层）
│       └── common/                             # client|event|config|util
├── <svc>-start/                               # 启动模块（启动类 + application.yml，唯依赖 adapter）
│   └── src/main/java/{base}/start/Application.java
├── Dockerfile  service.yaml  README.md
└── (各模块 src/test 放测试)
```

**模块依赖方向（编译期强制）**：
```
start → adapter → app → domain ← infrastructure
adapter → client ; consumer → client jar only
```

> 完整多模块相比 Light 的差异**仅在分层边界由"包级"提升为"模块级"**：四层内部包结构、对象隔离、命名约定、依赖规则全部相同，规则详见 `x-cola5-architecture.md`。

---

## 4. 关键约束速查

| 维度 | 规则 |
|------|------|
| 跨仓契约 | 单一真源在 `meta-sdie/packages/contracts/`；域仓消费生成 client，契约变更走全局 ADR |
| 前端跨模块 | 禁止 import 他模块内部；经 `shared/types` 或 store 解耦 |
| 后端分层 | `adapter→app→domain←infra`，domain/client 零业务依赖，adapter 不依赖 infra |
| 后端对象 | DTO∈client、Cmd/Qry/VO∈app、Entity/V/Event∈domain、DO/Response∈infra，转换集中到 Converter |
| 静态资源 | 前端 `public/`（固定 URL）vs `src/**/assets/`（哈希/内联）；模块私有资源放 `modules/<feature>/assets/` |
| 治理一致 | Gate 阈值（红线③）统一来自 `meta-sdie/.sdie/config.yaml`，各域 CI 远程拉取；模板为 meta 推送的「受控副本」+ `SYNCED-FROM` 漂移校验（见 §7） |

---

## 5. 命名约定（合并）

| 对象 | 规则 | 示例 |
|------|------|------|
| 治理仓 | 固定 | `meta-sdie` |
| 域仓 | `domain-<bounded-context>` | `<domain>` |
| 域内前端 | `apps/<web>` | `<web>` |
| 域内服务 | `services/<svc>` | `<svc>` |
| 前端模块/目录 | `kebab-case`；Vue 文件 `PascalCase`；组合函数 `use*` | `<feature>` / `<Feature>Profile.vue` / `use<Feature>.ts` |
| 后端契约 | `{Resource}Api` / `{Resource}{Action}DTO` | `<Resource>Api` / `<Resource>{Action}DTO` |
| 后端层/包 | COLA 约定（adapter/application/domain/infrastructure） | `<Resource>Controller` / `<Resource>{Action}CmdExe` / `<Resource>GatewayImpl` |
| 领域对象 | 实体无后缀、值对象 `V`、事件 `Event` | `<Resource>` / `<ValueObject>V` / `<Resource>SubmittedEvent` |
| 发布列车 | `train-vMAJOR.MINOR.PATCH` | `train-v1.0.0` |
| 迭代 | `iter-NNN` | `iter-001` |

---

## 6. 引用与权威源

- **组织 / 域仓骨架、跨仓治理、迭代模型**：`microservices-scaffolding-polyrepo.md`（范式 C）
- **前端细节（完整规则：命名/格式/语法/静态资源/测试）**：`vue3-dir-architecture.md`
- **后端细节（完整强制项：分层/依赖/Lombok/对象隔离）**：
  - `x-cola5-light-architecture.md`（默认，单模块 + 包级分层）
  - `x-cola5-architecture.md`（完整多模块备选）
- **SDIE 纪律唯一权威基准**：`SDIE-RACI-Matrix.md`；架构选型（红线②）、门禁阈值（红线③）须由对应人类角色审定。

> 本文由 AI 协作综合生成；结构事实以四份源文档为基准，决策类事项（域切分、契约格式、COLA 多模块 vs Light）须由 Tech Lead 审批（红线②）。

---

## 7. 模板治理：受控副本与漂移防控

> 本节回应 `<domain>*/.sdie/templates` 的价值前提——**本地副本必须有同步机制与漂移检测，否则会从「受控副本」退化为「未治理的复制负债」**。

### 7.1 为什么是本地副本而非子仓引用
- **仓级自洽 / CI 密闭性**：polyrepo 下各 `<domain>*` 是独立 git 仓，CI 作业不共享工作区；本地副本使域仓 CI 在生成/校验四阶段制品时**不依赖 meta-sdie 被 checkout**，构建可复现、密闭（hermetic）。
- **AI Coding 在仓闭环**：域仓内 Agent 生成本域 PRD / task-spec / test-strategy 时，在本仓内即可取用规范模板，无需跨仓 fetch meta，与 `docs/1-Spec…4-Evaluation` 同仓闭环、可追溯。
- **版本钉选 / 受控演进**：域可钉选某一版模板；meta 升级时按 PR 择机合入，而非被迫变更。

### 7.2 同步机制（权威源 → 域仓）
- 由 `meta-sdie/.sdie/scripts/sync-templates.sh` 负责：读取 meta 模板的**钉选版本**（commit/version），推送到各 `<domain>*/.sdie/templates/`，并自动向各域仓开「模板更新 PR」。
- 域仓**被动接收** PR，合入即完成同步；禁止域仓在副本上长期私自修改（如需本地定制，应先回流 meta 或走 fork 评审）。

### 7.3 漂移检测
- 每个域副本内置 `SYNCED-FROM` 元数据（记录 `meta-sdie@<commit/version>`）。
- 域 CI 在 `pre-commit`/流水线中比对：若本地模板与 meta 当前钉选版本偏差超阈值 → 告警，提示运行 `sync-templates.sh` 或合入待处理 PR。

### 7.4 与 `config.yaml` 的不对称说明（治理一致性口径）
- **模板**：创作期需在域仓内本地可用 → 故采用「受控副本」。
- **Gate 阈值（红线③）**：仅在评估/CI 期使用，且要求全局唯一真源 → 不复制，各域 CI **远程拉取** `meta-sdie/.sdie/config.yaml`。
- 两者不对称是刻意的：复制的是「高频创作脚手架」，引用的是「低频强一致约束」。

### 7.5 更新策略（红线② 审定）
- 模板结构/字段变更属治理变更，须由对应人类角色（Tech Lead / 架构负责人）审定，经 `sync-templates.sh` 统一下发；域仓不得自作主张改写副本内容。
