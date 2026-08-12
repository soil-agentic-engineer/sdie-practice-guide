## 元信息（meta）

```yaml
id: USM-<FEATURE>-001
title: 用户故事地图：<功能名>
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - PRD-<feature>.md
last_updated: 2026-08-12
```

# 用户故事地图：<功能名>

> 方法论：User Story Mapping（Jeff Patton，权威推荐）。四层结构见 `SDIE-Spec-Guide.md` §4.2。
> RACI：PM/PO = A；Dev = R（协助梳理）；SME / Tech Lead = C。
> 每条 story 须 trace 至 PRD §4 的 Impact（IMP-x）；未挂 Impact 的 story 视为范围外候选（见 PRD §2）。

## 主干（Backbone，左→右时序）

> 用户视角（Actor，来自 PRD §3）：<主要角色，如"买家">；off-stage：<合规/支付网关>（其行为目标见 PRD §4）。
> 地图围绕上述用户旅程构建：横轴 = 时序（左→右）、纵轴 = 优先级（上→下）。

| activity（主干活动） | step（用户任务） |
|----------------------|------------------|
| 浏览 | 进入列表页 / 打开详情 |
| 加购 | 查看加购按钮 / 点击加购 |
| 结算 | 确认订单 / 支付 |
| 管理订单 | 查看订单 / 取消 |

## 细节（Details，按优先级排序）

> 每条 story 须过 INVEST 校验（Independent / Negotiable / Valuable / Estimable / Small / Testable，Bill Wake 2003）。

| story（实现细节） | priority | backbone 挂接（activity/step） | impact（来自 PRD §4） |
|-------------------|----------|-------------------------------|------------------------|
| <高优先故事> | P0 | 加购 / 点击加购 | IMP-1（买家即时下单） |
| <低优先故事> | P2 | 结算 / 确认订单 | IMP-2（客服免转人工） |

## 发布切片（Release slices，横向分割）

- MVP（顶部）：...
- 未来增强（底部）：...

## 暂不做（Out-of-scope / deferred）

- `deferred`: [...]

## 版本历史

| 版本 | 日期 | 变更摘要 | 关联 ADR |
|------|------|----------|----------|
| 1.0.0 | 2026-08-06 | 初始 baseline：四层结构（Backbone/Details/Release slices/Out-of-scope）+ 方法论溯源 + RACI | — |
| 1.1.0 | 2026-08-12 | MINOR：补 impact trace 列（对应 PRD §4）、actor 用户视角、story 挂接列与 INVEST 注记；补版本历史段落（对齐 SDIE-Spec-Guide §6.5） | — |
