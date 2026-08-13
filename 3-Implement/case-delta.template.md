## 元信息（meta）

```yaml
id: CASE-DELTA-<TASK-ID>
title: <功能>Case Delta 报告（测试删/禁用审计）
status: review
phase: Implement
owner: dev-<名>            # 开发工程师（R）；授权人=提交 trailer 的人类（⑥）
related_docs:
  - PR-<TASK-ID>
last_updated: 2026-08-06
```

# Case Delta 报告：<功能>（<TASK-ID>）

> **用途**：证明本次变更**没有偷偷删除/禁用测试**（Gate 3 准入项之一）。
> **不可委托 ⑥**：任何测试删除/禁用必须由提交 commit trailer 的人类显式授权；
> QA 在 ④·验证 中审本报告的真实性。

## 统计
- 删除用例: 0
- 禁用用例(skip/ignore): 0
- 新增用例: N
- 未授权改动: 无

## 明细（如有删/禁用，逐条列出授权）
| 用例 | 操作 | 授权人(trailer) | 理由 | 关联 ADR |
|------|------|----------------|------|----------|
| （示例）UT-Order-07 | disable | dev-zhang | 依赖废弃 mock | ADR-xxx |

## 结论
- [x] 无未授权删/禁用 → Gate 3「Case Delta 干净」通过
- [ ] 存在未授权删/禁用 → Gate 3 卡住，须补 trailer 或恢复用例
