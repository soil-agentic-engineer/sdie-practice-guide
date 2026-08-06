---
id: ADR-DESIGN-000
title: <架构决策标题>
status: draft            # draft → review → baseline → change → superseded
phase: Design
owner: techlead-<名>     # Tech Lead / 架构负责人
related_docs:
  - SDIE-Design-Guide.md
  - TASK-<feature>.yaml
last_updated: 2026-08-06
---

# ADR-DESIGN-000：<架构决策标题>

> **红线**：ADR 的 Status / Decision 由 **Tech Lead 定稿（不可委托 ②）**，Design Agent 仅可起草；
> ADR 不可原地涂改，替代时标 `Superseded by`。本文件是 Design 阶段 Gate 2 的核心证据。

## Status
Proposed | Accepted | Superseded by ADR-DESIGN-00Y

## Context
什么力量/约束迫使此决策（业务目标、合规、技术债、性能边界…）。

## Decision
我们决定：<具体架构选型与理由，如引入领域事件解耦加购/结算/库存>。

## Consequences
- 正向：<解耦、可独立扩容…>
- 负向：<需消息可靠投递与幂等设计…>
- 中性：<>

## Alternatives
被否决的备选及其理由（如同步 RPC：耦合高、超时传播）。
