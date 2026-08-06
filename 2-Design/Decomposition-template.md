---
id: DECOMP-<FEATURE>-000
title: <功能>原子分解方案
status: draft
phase: Design
owner: dev-<名>            # 开发工程师 Task Owner
related_docs:
  - TASK-<feature>.yaml
  - ADR-DESIGN-000
last_updated: 2026-08-06
---

# <功能>原子分解方案

> **作者**：Dev (Task Owner)（R）；**问责/审批**：Tech Lead（Gate 2）。
> 目标：把 Task Spec 拆为 **Agent 可独立完成**的原子任务，并映射验收与上下文范围。

## 分解总览
| task_id | title | agent_assignable | depends_on | acceptance_ref | context_scope |
|---------|-------|------------------|------------|----------------|---------------|
| DECOMP-001 | 购物车加入商品接口 | true | [] | AC-1, AC-2 | src/checkout/*, docs/design/ADR-012 |
| DECOMP-002 | 库存校验守卫 | true | [DECOMP-001] | AC-2 | src/inventory/* |
| DECOMP-003 | 并发超卖防护 | false | [DECOMP-001,002] | AC-红卡 | 需人类写骨架 |

## 任务间依赖与验收映射
- DECOMP-001 是入口；DECOMP-002 提供库存校验；DECOMP-003 处理并发边界（依赖前两者）。
- 每个原子任务的 `acceptance_ref` 必须对应 Spec 阶段 `acceptance_criteria` 编号，禁止自创验收。

## 备注
- `agent_assignable=false` 的任务需人类先写骨架，Agent 仅补实现。
- 上下文范围（context_scope）需与「上下文注入策略」模板保持一致。
