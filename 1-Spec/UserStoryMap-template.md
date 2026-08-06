---
id: USM-<FEATURE>-001
title: 用户故事地图：<功能名>
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - PRD-<feature>.md
last_updated: 2026-08-06
---

# 用户故事地图：<功能名>

> 方法论：User Story Mapping（Jeff Patton，权威推荐）。四层结构见 `SDIE-Spec-Guide.md` §6.2。
> RACI：PM/PO = A；Dev = R（协助梳理）；SME / Tech Lead = C。

## 主干（Backbone，左→右时序）

| activity（主干活动） | step（用户任务） |
|----------------------|------------------|
| 浏览 | 进入列表页 / 打开详情 |
| 加购 | 查看加购按钮 / 点击加购 |
| 结算 | 确认订单 / 支付 |
| 管理订单 | 查看订单 / 取消 |

## 细节（Details，按优先级排序）

| story（实现细节） | priority |
|-------------------|----------|
| <高优先故事> | P0 |
| <低优先故事> | P2 |

## 发布切片（Release slices，横向分割）

- MVP（顶部）：...
- 未来增强（底部）：...

## 暂不做（Out-of-scope / deferred）

- `deferred`: [...]
