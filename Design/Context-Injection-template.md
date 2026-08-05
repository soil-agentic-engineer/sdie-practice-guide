---
id: CTX-INJECT-<FEATURE>
title: <功能>上下文注入策略
status: draft
phase: Design
owner: techlead-<名>      # Tech Lead（R/A）
related_docs:
  - DECOMP-<FEATURE>-000
last_updated: 2026-08-06
---

# <功能>上下文注入策略

> **作者/问责**：Tech Lead（R/A，Gate 2）。决定 Agent 在执行原子任务时能读取哪些文件/文档，
> 防止越权读取或上下文污染。需与「原子分解方案」的 context_scope 保持一致。

## 允许读取（allow_read）
- src/checkout/*
- docs/design/ADR-012
- docs/specs/task-specs/TASK-<feature>.yaml

## 禁止读取（deny_read）
- secrets/ 、 .env 、 其他租户模块
- 与当前任务无关的历史分支

## 注入方式（inject_via）
- PR 描述 / 系统提示 / 文件引用（任选其一或组合）

## 人类复核点（verify）
- Task Owner 审 Agent 输出是否越界读取禁止路径。
- 任何范围变更需更新本文件并重新过 Gate 2（Harness 维护归 Dev+TL ⑩）。
