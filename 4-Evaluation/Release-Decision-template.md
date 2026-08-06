---
id: RELEASE-DECISION-<ITER>
title: <项目>发布决策与回滚预案
status: review
phase: Evaluation
owner: qa-<名>              # 测试架构师 / QA（R/A，⑧）
related_docs:
  - QUALITY-DASH-<ITER>
  - ADR-<release>
last_updated: 2026-08-06
---

# 发布决策与回滚预案：<项目>

> **问责**：QA（A，⑧ 发布决策与回滚预案）。**回滚的质量触发条件与回滚验收标准由 QA 定，
> 执行机制与 Tech Lead（及 SRE，若有）共担**。本决策不得委托给 Agent。

## 发布决策
- 决策: Go / No-Go
- 依据: 质量看板指标达标（变异分≥__%、0 高危、缺陷率<__/kloc）

## 质量触发条件（QA 定）
- 变异分 ≥ __% 且 0 高危 且 缺陷率 < __/kloc

## 回滚验收标准（QA 定）
- 5 分钟内恢复至 vX.Y.(Z-1)
- 核心链路可用率 ≥ 99.9%

## 执行机制（共担）
- Tech Lead + SRE：蓝绿/灰度发布与回滚执行
- 通知链：QA → PO（知会，非 A）→ 全员

## 不可委托声明
- 本发布决策 A 归 QA（⑧）；安全判定另归安全/红队（⑤）。
