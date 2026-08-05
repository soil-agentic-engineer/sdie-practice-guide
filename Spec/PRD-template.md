---
id: PRD-<FEATURE>-001
title: PRD：<功能名>
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - UserStoryMap-<feature>.md
  - AC-<feature>.md
  - TASK-<feature>.yaml
last_updated: 2026-08-06
---

# PRD：<功能名>

> **RACI**：PM/PO = A/R（① 业务需求与验收语义拍板，不可委托）；SME / Tech Lead / QA = C；安全/红队、Reviewer = I。
> **Gate 1**：本 PRD 需 `status=baseline` 且经 PM 签 Gate 1、QA 复核验收可测，方可进入 Design（§6）。
> 正文结构依据 `SDIE-Spec-Guide.md` §6.1；方法论标注见 §8.2。

## 1. 背景与目标（Business Goal）
   - 目标（建议用 Impact Mapping 的 Goal 写法，SMART，权威推荐）：
     "在 Q3 将<某指标>从 X% 降到 Y%"

## 2. 范围（Scope）
   - in_scope: [...]
   - out_of_scope: [...]   ← PM 在 Gate 1 标注，防范围蔓延（§3.5.1 #5）

## 3. 用户与角色（Actors）
   - 主要角色：...；次要角色：...；off-stage：合规/支付网关

## 4. 用户故事地图摘要（链接 story-map 文档）
   - 见 `UserStoryMap-<feature>.md`

## 5. 验收标准集（嵌入 acceptance_criteria）
   - 见 `AC-<feature>.md` 或下方章节

## 6. 非目标与假设（Non-goals / Assumptions）
   - ...

## 7. 优先级（MoSCoW + KANO 标注，权威推荐）
   - Must / Should / Could / Won't；Basic / Performance / Excitement
