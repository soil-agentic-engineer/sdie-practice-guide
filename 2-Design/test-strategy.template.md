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
last_updated: 2026-08-06
```

# <功能>测试策略与门禁阈值草案

> **作者**：QA（C，草案）；**阈值最终设定**：Tech Lead + QA 共定（不可委托 ③）。
> 本草案是 Gate 2 的咨询输入，最终值写入 Harness（scripts/validate-*.sh）与 Jenkinsfile。

## 测试层级（Test Levels）与分级占比（草案）
> 测试层级按集成范围划分：单元 / 集成 / 系统(端到端) / 验收。
> 单元·集成·系统采用金字塔占比分布；验收为跨阶段层级（语义在 Spec、执行在 Gate 3、签收在 Gate 4），不重复写用例、不计入金字塔条数。

| 测试层级 | 占比（草案） | 说明 / 落点 |
|----------|-------------|-------------|
| 单元测试(unit / component) | 70% | 亚秒级、确定性、不触网/库；TDD 红-绿-重构 |
| 集成测试(integration) | 20% | WireMock/本地 fake 窄集成；一次测一个外部边界 |
| 系统测试(system / e2e) | 10% | 仅覆盖核心旅程（如 搜索→加购→结算） |
| 验收测试(acceptance) | —（跨阶段） | 可执行形态＝`acceptance_criteria` 的 AC-N 经 `behavior-checklist.yaml` 的 `maps_to` 转自动化行为/端到端测试，Gate 3 随 CI 执行须 100% 通过；人类签收＝Gate 4 由 QA(⑧) 发布放行 + PM(C) 业务价值确认，并以 `coverage_of_ac` 度量验收覆盖 |

## 门禁阈值草案（待共定后写入 Harness）
- 覆盖率(coverage): ≥ 80%
- 变异分(mutation / PITest): ≥ 60%   ← ④·验证 核心证据，非仅覆盖率
- 安全: 0 高危 (0 high)

## 非功能测试类型（可选）
> 以下质量属性测试类型**默认可选**，按模块风险（risk-matrix）与业务关键度启用；
> 启用项须在本策略显式勾选，并写入对应门禁/基线，避免"悄悄漏测"。

- [ ] **性能 / 负载 / 压力**：启用时定义基线（P95 延迟、吞吐、并发上限）与压测场景；risk=high 建议启用
- [ ] **可靠性 / 混沌 / 容错**：启用时定义混沌演练（节点失效、网络分区）与恢复验收（对齐 Gate 4 回滚预案）
- [ ] **兼容性 / 跨浏览器 / 跨端**：启用时定义矩阵（OS / 浏览器 / 分辨率）
- [ ] **易用性 / 可访问性（a11y）**：启用时定义核查清单（如 WCAG 基线）
- [ ] **安全测试（作为类型）**：除门禁"0 高危"外，启用时列对抗演练红卡场景（提示注入 / 越权 / 越界读 secrets），详见 `security-design.template.md`（⑤）

## 测试数据方案
- 用影子库 + 工厂方法，禁止生产数据入测试。
- 并发/边界用例（如超卖）需在 Implement 前预埋。

## 共定记录
- [ ] Tech Lead 已确认阈值（含非功能启用项）
- [ ] QA 已确认阈值（含非功能启用项）
- 最终阈值见 Harness 配置（落位 scripts/ 与 Jenkinsfile，由 Dev+TL 维护 ⑩）
