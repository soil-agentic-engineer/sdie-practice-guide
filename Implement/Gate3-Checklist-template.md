---
id: GATE3-<TASK-ID>
title: <功能>Gate 3 准入自检
status: review
phase: Implement
owner: dev-<名>            # 开发工程师（R 侧发起）
related_docs:
  - PR-<TASK-ID>
  - REVIEW-<TASK-ID>
last_updated: 2026-08-06
---

# Gate 3 准入自检：<功能>（<TASK-ID>）

> **门禁**：Lint 0 违规、测试 100% 通过、安全 0 高危、Case Delta 无未授权删/禁用、PR 标签正确、Review 通过。
> **三 A 责任链**（按序）：QA（④·验证）→ Reviewer（④·签字）→ PO（⑦ 收货合并）。
> 自动化门禁只能证明"格式合规"，**不能证明"代码正确"**，所以 ④ 两项必须由人。

## 自动化准入（机器）
- [ ] Lint 0 违规
- [ ] 测试 100% 通过
- [ ] 安全扫描 0 高危
- [ ] Case Delta 无未授权删/禁用
- [ ] PR 标签正确（type/scope）

## 人类问责链（不可委托）
- [ ] **QA（④·验证）**：覆盖率/变异分(PITest)达标、Case Delta 真实 → 证据可信
- [ ] **Reviewer（④·签字）**：消费 QA 证据，技术批准签字 → 代码正确
- [ ] **PO（⑦ 收货合并）**：技术批准后在 PR 上确认收货 → 业务接受

## 结论
- [ ] 全部通过 → 进入 Evaluation（Gate 4 由 QA 审批）
- [ ] 任一未过 → 退回对应环节
