## 元信息（meta）

```yaml
id: TEST-STRATEGY-<FEATURE>
title: <功能>测试策略与门禁阈值草案
status: draft
phase: Design
owner: qa-<名>              # 测试架构师 / QA（C，草案）
related_docs:
  - SDIE-Design-Guide.md
  - acceptance-criteria.template.md   # 验收层级来源（Spec）
  - acceptance-result.template.md     # 验收结果判定（Evaluation，Gate 4）
last_updated: 2026-08-06
```

# <功能>测试策略与门禁阈值草案

> **作者**：QA（C，草案）；**阈值最终设定**：Tech Lead + QA 共定（不可委托 ③）。
> 本草案是 Gate 2 的咨询输入，最终值写入 Harness（scripts/validate-*.sh）与 Jenkinsfile。
> **组织原则**：测试层级（level）为脊、架构端（layer：前端 / 后端 / 跨端）为列——采用「层级 × 端」矩阵，
> **不拆成「前端/后端」两份文档**。理由：e2e 与验收本质跨端，拆文档会让这两层成孤儿层且破坏 SDIE「单 feature 单制品」约定。

## 测试层级（Test Levels）与分级占比（草案）
> 测试层级按集成范围划分：单元 / 集成 / 系统(端到端) / 验收。
> 单元·集成·系统采用金字塔占比分布；验收为跨阶段层级（语义在 Spec、执行在 Gate 3、签收在 Gate 4），不重复写用例、不计入金字塔条数。

| 测试层级 | 占比（草案） | 适用端 | 说明 / 落点 |
|----------|-------------|--------|-------------|
| 单元测试(unit / component) | 70% | 前端 + 后端 | 前端：组件 / RTL（Jest / Vitest），mock 依赖；后端：服务方法（JUnit / pytest），mock 依赖；亚秒级、确定性、不触网/库；TDD 红-绿-重构 |
| 集成测试(integration) | 20% | 后端为主（前端为辅） | 后端：DB/JPA、缓存、MQ、外部 HTTP（WireMock / Testcontainers）、**CDC 契约（Pact）**；前端：多组件+状态+MSW mock 网络（RTL + MSW），验用户交互 |
| 系统测试(system / e2e) | 10% | 跨端 | 仅覆盖核心旅程（如 搜索→加购→结算），Playwright / Cypress 穿越前后端 |
| 验收测试(acceptance) | —（跨阶段） | 跨端 | 可执行形态＝`acceptance_criteria` 的 AC-N 经 `behavior-checklist.yaml` 的 `maps_to` 转自动化行为/端到端测试（前端行为 + 后端行为共同满足），Gate 3 随 CI 执行须 100% 通过；人类签收＝Gate 4 由 QA(⑧) 发布放行 + PM(C) 业务价值确认，并以 `coverage_of_ac` 度量验收覆盖 |

## 门禁阈值草案（待共定后写入 Harness）
- 覆盖率(coverage): ≥ 80%
  - 前端：Istanbul / v8，**e2e 不计入行覆盖**（走真实浏览器，只验走通）
  - 后端：JaCoCo / Cobertura，**含集成（Testcontainers 真 DB 才算数）**
- 变异分(mutation / PITest): ≥ 60%   ← ④·验证 核心证据，非仅覆盖率
  - 后端：PITest（Java 成熟）
  - 前端：单测变异工具可选（成熟工具有限），可暂以"子集"评估
- 安全: 0 高危 (0 high)
  - 前端：依赖审计 + SAST
  - 后端：SAST + DAST
- 契约验证(contract): 通过（Provider CI 强制，见下方「契约测试使用约定」）

## 契约测试使用约定（跨服务 / RPC 接口兼容性）
> 详细方法与规则见 `0-References/contract-testing.md`（消费者驱动契约 / Pact）。
- 适用清单：列出本功能涉及的跨团队/跨模块服务对与协议（HTTP / gRPC / Kafka 等）。
- CDC 顺序：Consumer 先写 expectation 并发布契约 → Provider 拉取后用**真实服务**验证（Provider 侧禁止 mock）。
- 契约仓库：统一存于 Pact Broker / `docs/contracts/`，版本化，破坏性变更走 SemVer MAJOR（联动 SDIE-Design-Guide.md §7.3）。
- 协议支持：gRPC 用 `pact-protobuf-plugin`（proto 即契约）；异步消息用 Message Pact；Thrift/Dubbo 需自写方案并记 ADR。
- 归属：契约测试归入「集成层」占比内，非独立条数；与 RACI 联动——QA(C) 拟策略、Dev(R) 落用例、Tech Lead(A) 审契约级破坏性变更（②）。

## 非功能测试类型（可选）
> 以下质量属性测试类型**默认可选**，按模块风险（risk-matrix）与业务关键度启用；
> 启用项须在本策略显式勾选，并写入对应门禁/基线，避免"悄悄漏测"。

- [ ] **性能 / 负载 / 压力**：启用时定义基线（P95 延迟、吞吐、并发上限）与压测场景；risk=high 建议启用
- [ ] **可靠性 / 混沌 / 容错**：启用时定义混沌演练（节点失效、网络分区）与恢复验收（对齐 Gate 4 回滚预案）
- [ ] **兼容性 / 跨浏览器 / 跨端**：启用时定义矩阵（OS / 浏览器 / 分辨率）；偏前端
- [ ] **易用性 / 可访问性（a11y）**：启用时定义核查清单（如 WCAG 基线）；偏前端
- [ ] **安全测试（作为类型）**：除门禁"0 高危"外，启用时列对抗演练红卡场景（提示注入 / 越权 / 越界读 secrets），详见 `security-design.template.md`（⑤）

## 测试覆盖与质量打分（草案）
> 覆盖与打分遵循「分层门禁 + 看板」，不把质量压成单一总分。覆盖率仅证明"代码被执行"，
> 不证明"测试有效"——以**变异分(PITest)**作核心有效性证据（呼应 ④·验证 红线）。

### 覆盖率阶梯（按层取指标）
| 测试层级 | 前端工具 / 度量 | 后端工具 / 度量 | 度量重点 |
|----------|----------------|----------------|----------|
| 单元 | v8 / Istanbul | JaCoCo / Cobertura | 语句 C0 + 分支 C1（关键逻辑 ≥ C1） |
| 集成 | RTL + MSW | Testcontainers 真 DB | 协作路径分支覆盖 |
| 系统 / E2E | 不计入行覆盖（走真实浏览器） | 不计入行覆盖 | 走通即可，不追覆盖率 |
| 验收 | `coverage_of_ac`（应 = 1.0） | 同左 | 每条 AC-N 是否被 behavior 覆盖 |

### 质量打分四层（独立门禁 · AND 组合，非加权求和）
> **方案 B · 分层独立门禁**：四层各自独立评分、各自独立门禁；放行条件是**全部通过（AND）**，不是加权合成一个总分。
> 量纲不可加（变异分% / 覆盖率% / `coverage_of_ac` 比率 / DORA 频率 彼此不可通约），单分无物理意义且会掩盖弱层（"高分低质"陷阱——如一半 AC 未覆盖却被高分稀释）。
> 各层门禁分属不同 Gate、不同问责人（变异分 / 覆盖率 / 安全 → Gate 3；`coverage_of_ac` → Gate 4；DORA 仅看板趋势），合并成单分会抹掉"硬门槛 vs 趋势参考"的差别，违背 ④·验证 红线（证据不得自报自批）。

| 维度 | 指标 | 阈值（草案） | 门禁 |
|------|------|--------------|------|
| 结构有效 | 变异分(mutation / PITest) | ≥ 60% | Gate 3 ④·验证 核心 |
| 结构完整 | 覆盖率(coverage) | ≥ 80% | Gate 3 |
| 验收覆盖 | `coverage_of_ac` | = 1.0（建议由 per-AC `result` 派生，禁手填） | Gate 4 |
| 安全 | 高危数 | 0 高危 | Gate 3 / 4 ⑤ |
| 效率 | DORA 四指标（部署频率 / 前置时间 / 变更失败率 / MTTR） | 对标 elite | 看板趋势 |

> **单分定位（非权威沟通物）**：若对外汇报需要单一数字，仅可将四层门禁结果做加权展示（如 变异分 40% + `coverage_of_ac` 30% + 安全 20% + DORA 10%），
> 但**该单分仅为沟通摘要，不得作为放行输入**；放行只看各层门禁全过（AND），且须先过 Gate 3 自动化门禁（Lint0 / 测试 100% / 安全 0 / Case Delta 干净）。

## 测试数据方案
- 用影子库 + 工厂方法，禁止生产数据入测试。
- 并发/边界用例（如超卖）需在 Implement 前预埋。

## 共定记录
- [ ] Tech Lead 已确认阈值（含非功能启用项）
- [ ] QA 已确认阈值（含非功能启用项）
- 最终阈值见 Harness 配置（落位 scripts/ 与 Jenkinsfile，由 Dev+TL 维护 ⑩）
