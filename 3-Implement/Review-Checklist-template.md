---
id: REVIEW-<TASK-ID>
title: <功能>Review 清单（④·签字 依据）
status: review
phase: Implement
owner: reviewer-<名>       # 评审官 Reviewer（A，④·签字）
related_docs:
  - PR-<TASK-ID>
  - CASE-DELTA-<TASK-ID>
last_updated: 2026-08-06
---

# Review 清单：<功能>（<TASK-ID>）

> **问责**：Reviewer（A，④·签字）。**Review Agent 仅给建议，不得代签字（④ 红线）**。
> 签字前必须**消费 QA 的 ④·验证 结论**（覆盖率/变异分/Case Delta）再确认"代码技术上可合并"。

## 签字前逐项确认
- [ ] 正确性：实现满足 `acceptance_criteria`（AC-1/AC-2/并发红卡）
- [ ] 安全：无越权/注入；未改安全配置（⑤）
- [ ] 规范：命名/分层符合 Design 契约（ADR / 分解方案）
- [ ] 证据：QA 变异分达标、Case Delta 干净（消费 ④·验证 结论）
- [ ] 自验：Dev 本地 build/test/lint 通过（⑨ 人类判定）

## 技术批准签字（④·签字）
- [ ] 「本人承担最终质量责任」→ **签字**：Reviewer <名> @ <日期>
- 签字后是否真进主干，仍由 PO 决定（⑦ 收货合并）。
