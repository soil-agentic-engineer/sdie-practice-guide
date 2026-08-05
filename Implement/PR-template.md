---
id: PR-<TASK-ID>
title: <功能>实现 PR
status: review             # draft → review → baseline → change → superseded
phase: Implement
owner: dev-<名>            # 开发工程师 Task Owner
related_docs:
  - TASK-<feature>.yaml
  - DECOMP-<feature>-001
last_updated: 2026-08-06
---

# PR：<功能>实现（<TASK-ID>）

> **作者**：Dev (Task Owner)（R，注入上下文 + 审 Agent 输出 + 自验，⑨ 判定不可委托）。
> **责任链**：QA（④·验证）→ Reviewer（④·签字）→ PO（⑦ 收货合并）。

## 自验结果（Dev 本地）
- build: pass | lint: 0 violation | test: 100% pass | 安全扫描: 0 high

## 变更摘要
- 新增/修改：<列表>
- 测试：保留全部 N 条用例（无删/禁用）

## 测试删/禁用声明（不可委托 ⑥）
- 本次无测试删除/禁用。
- 若曾删除/禁用，必须显式授权（trailer 由人类提交）：
  `Test-Disable-Authorization: <人类名> <理由: 该用例依赖已废弃 mock，见 ADR-xxx>`
- **无 trailer 的测试删/禁用 = Gate 3 卡住。**

## 关联
- 行为清单：Behavior-Checklist-<TASK-ID>.yaml
- Case Delta：Case-Delta-<TASK-ID>.md
