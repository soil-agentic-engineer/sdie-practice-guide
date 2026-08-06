---
id: QUALITY-DASH-<ITER>
title: <项目>-<迭代>质量看板
status: review
phase: Evaluation
owner: qa-<名>              # 测试架构师 / QA（R/A，⑧）
related_docs:
  - RELEASE-DECISION-<ITER>
  - SDIE-Evaluation-Guide.md
last_updated: 2026-08-06
---

# 质量看板：<项目>-<迭代>

> **作者/问责**：QA（R/A，⑧ 发布放行）。本看板是 Gate 4「质量/效率指标达标」的核心证据。
> 注意：覆盖率绿条不等于质量证据——必须看**变异分(Case Delta 干净)** 与缺陷逃逸。

## 质量指标
- 覆盖率(line): __% | 分支: __% | **变异分(mutation): __%** ← ④·验证 延续证据
- 缺陷率: __/kloc | 逃逸缺陷: __

## 效率指标（DORA）
- 部署频率: __ | 交付前置时间: __ | 变更失败率: __% | MTTR: __

## 趋势与关注
- 较上迭代变化：变异分 +__pt（关注点：<模块>）
- 阻塞项：<列出影响发布的事项>
