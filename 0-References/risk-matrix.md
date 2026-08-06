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

## 评分量表（1–5 锚定）

### Probability（发生概率）
| 分值 | 等级 | 定性描述 | 经验区间* |
|----|----|----|----|
| 1 | Very Low（很低） | 极难发生，仅在极端组合下出现 | ≤10% |
| 2 | Low（低） | 偶发，正常流程下不易触发 | 11–30% |
| 3 | Medium（中） | 可能发生，存在明确诱因 | 31–50% |
| 4 | High（高） | 较易发生，条件具备即触发 | 51–80% |
| 5 | Very High（很高） | 几乎必然发生 | >80% |

### Impact（影响严重度）
针对"不确定性对目标（交付/正确性/安全/合规）的影响"，不限于成本维度：
| 分值 | 等级 | 定性描述 | 后果示例 |
|----|----|----|----|
| 1 | Negligible（可忽略） | 几乎无影响，可轻易补救 | 局部样式偏差，有现成替代 |
| 2 | Minor（轻微） | 局部影响，有低成本回退 | 单模块小返工，不波及排期 |
| 3 | Moderate（中等） | 影响主要功能/排期，需一定返工 | 核心路径延迟 1–2 天 |
| 4 | Major（严重） | 影响发布/安全基线，需显著返工 | 发布阻断或绕过安全基线 |
| 5 | Severe（致命） | 导致发布失败或安全事故 | 数据泄露 / 线上事故 |

> \* 经验区间仅作锚定参考；SDIE 中使用时以团队对"本任务在目标上的偏离程度"的共识为准，不必机械套用百分比。

## 三档阈值映射（score = Probability × Impact，1–25）

| 分值区间 | 枚举 | 等级 | 处置（SDIE 联动） |
|----|----|----|----|
| 1–6 | low | 低 | 常规处理，Agent 可自主；无需额外审查 |
| 7–12 | med | 中 | Dev+R 自审；Gate 2 关注；纳入常规验证 |
| 13–25 | high | 高 | Gate 2 优先审、Implement 优先验证、加强 Review |
| └ 16–25 | （high 子集） | 危急 | 触发升级：联动 ⑤ 安全红线、PO/安全工程师介入评估 |

> 阈值分区由团队 Risk Management Plan 自定，以上分区为**参考默认**，非通用硬性标准。实战中可按项目风险偏好收紧（如 high 起点上移至 15）或放宽。

## 在 SDIE 中的用法（建议，非强制）
- DECOMP task 可加 `risk` 字段：`probability×impact` 枚举（low / med / high）或 1–25 分值。
- 高 risk task → Gate 2 优先审、Implement 优先验证、加强 Review（呼应 Gate 3 串联）。
- 安全相关风险联动 ⑤：context_scope 不得读 `secrets/`。
- 与 `agent_assignable:false`（暗示高复杂度需人类写骨架）互补"后果严重度"维度。

## 权威出处
- Project Management Institute. "PMBOK® Guide – Seventh Edition".
- ISO 31000:2018 "Risk management — Guidelines".
