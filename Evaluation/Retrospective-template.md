---
id: RETRO-<ITER>
title: <项目>迭代回顾
status: review
phase: Evaluation
owner: reviewer-<名>        # 评审官 Reviewer（C）牵头
related_docs:
  - RELEASE-DECISION-<ITER>
  - EVAL-CAP-<ITER>
last_updated: 2026-08-06
---

# 迭代回顾（Retrospective）：<项目>

> **牵头**：Reviewer（C）；**目标**：反馈 Agent 输出质量问题，推动 Harness 规则迭代（⑩）。
> 采用无责复盘（Postmortem）文化，聚焦流程改进而非追责。

## 做得好（Keep）
- Gate 3 三 A 串联顺畅，Case Delta 零违规
- 变异分较上迭代提升

## 待改进（Problem / Try）
- 并发超卖边界在 Implement 才暴露 → 应在 Design 预埋（关联 ADR-yyy）
- Agent 某类幻觉率偏高 → 补充上下文注入规则

## 行动项（Action）
| 行动 | 负责人 | 关联红线 | 截止 |
|------|--------|----------|------|
| 更新 AGENTS.md 上下文策略 | Dev+TL | ⑩ | <日期> |
| 新增并发测试模板 | QA | ③ | <日期> |

## 闭环
- 行动项须落实为 Harness 更新（⑩，Dev+TL）或 ADR，方可关闭本迭代。
