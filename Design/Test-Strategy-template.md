---
id: TEST-STRATEGY-<FEATURE>
title: <功能>测试策略与门禁阈值草案
status: draft
phase: Design
owner: qa-<名>              # 测试架构师 / QA（C，草案）
related_docs:
  - SDIE-Design-Guide.md
  - ADR-DESIGN-000
last_updated: 2026-08-06
---

# <功能>测试策略与门禁阈值草案

> **作者**：QA（C，草案）；**阈值最终设定**：Tech Lead + QA 共定（不可委托 ③）。
> 本草案是 Gate 2 的咨询输入，最终值写入 Harness（scripts/validate-*.sh）与 Jenkinsfile。

## 测试分级占比（草案）
- 单元测试(unit): 70% | 集成(integration): 20% | 端到端(e2e): 10%

## 门禁阈值草案（待共定后写入 Harness）
- 覆盖率(coverage): ≥ 80%
- 变异分(mutation / PITest): ≥ 60%   ← ④·验证 核心证据，非仅覆盖率
- 安全: 0 高危 (0 high)

## 测试数据方案
- 用影子库 + 工厂方法，禁止生产数据入测试。
- 并发/边界用例（如超卖）需在 Implement 前预埋。

## 共定记录
- [ ] Tech Lead 已确认阈值
- [ ] QA 已确认阈值
- 最终阈值见 Harness 配置（落位 scripts/ 与 Jenkinsfile，由 Dev+TL 维护 ⑩）
