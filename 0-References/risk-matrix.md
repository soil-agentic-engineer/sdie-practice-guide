# Risk Matrix（概率×影响）— 定性风险分析

> 引用来源：PMI PMBOK Guide 7th；ISO 31000:2018（风险管理原则）
> 在 SDIE 中引用位置：Decomposition 讨论（task 级 `risk` 字段）；联动 ⑤ 安全
> 用途：对原子任务标记威胁等级，辅助 Gate 2 优先审查与 Implement 优先验证。

## 定义
- **Risk（风险）**："the effect of uncertainty on objectives"（不确定性对目标的影响）—— PMBOK 7th / ISO 31000:2018。
- **定性风险分析**：用 **Probability(1–5) × Impact(1–5)** 矩阵，得分 1–25，分低 / 中 / 高 / 危急区。

## 核心要素
- 概率与影响均 1–5 评分，乘积定位风险等级。
- 应对阈值由 Risk Management Plan 定义（非通用标准）。

## 在 SDIE 中的用法（建议，非强制）
- DECOMP task 可加 `risk` 字段：`probability×impact` 枚举（low / med / high）或 1–25 分值。
- 高 risk task → Gate 2 优先审、Implement 优先验证、加强 Review（呼应 Gate 3 串联）。
- 安全相关风险联动 ⑤：context_scope 不得读 `secrets/`。
- 与 `agent_assignable:false`（暗示高复杂度需人类写骨架）互补"后果严重度"维度。

## 权威出处
- Project Management Institute. "PMBOK® Guide – Seventh Edition".
- ISO 31000:2018 "Risk management — Guidelines".
