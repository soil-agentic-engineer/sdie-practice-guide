---
id: AC-<FEATURE>-001
title: 验收标准集：<功能名>
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - PRD-<feature>.md
  - TASK-<feature>.yaml
last_updated: 2026-08-06
---

# 验收标准集（acceptance_criteria）：<功能名>

> **红线（不可委托 ①）**：逐条由 PM/PO 撰写（R），SME 审语义（C）、QA 审可测性（C）；
> Agent 不得自创领域规则或伪边界（§5 / §3.5.1）。
> 写法：BDD / Given-When-Then / Gherkin（Dan North / Cucumber，权威推荐）；辅以 Example Mapping（25 分钟，三 amigos）。
> 依据 `SDIE-Spec-Guide.md` §6.3。

## 正面示例（可测、具体、无歧义）

```
AC-1: 当用户在商品详情页点击"加入购物车"且库存>0 时，
      系统应在 500ms 内将商品写入购物车，并展示 toast"已加入购物车"。
AC-2: 当库存=0 时，按钮置灰并显示"已售罄"，点击不触发任何写操作。
```

## 反面示例（不可测 / 伪边界，SME 须拦截）

```
✗ "系统应快速响应用户操作。"        ← 无量化阈值，不可测
✗ "购物车应正常工作。"             ← 无行为定义
✗ "按钮应在合适的时候禁用。"       ← "合适的时候"是伪边界，需 SME 明确
```

## BDD / Gherkin 写法

```gherkin
Feature: 加入购物车
  Scenario: 库存充足时加入成功
    Given 商品 P 当前库存为 5
    When  用户在商品详情页点击"加入购物车"
    Then  购物车中出现 1 件 P
    And   页面展示 toast "已加入购物车"
```

## Example Mapping 收口（25 分钟 / 三 amigos）

- 黄卡（故事）：作为<角色>，我能<能力>
- 蓝卡（规则）：<业务规则 1>；<业务规则 2>
- 绿卡（例子）：<具体例子>
- 红卡（问题）：<待澄清项，记录为待澄清，不阻塞主流程>
