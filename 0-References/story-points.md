# Story Points / Planning Poker — 相对估算

> 引用来源：Mike Cohn (Agile Estimating and Planning, 2005)；Ron Jeffries 等 (XP)；James Grenning (Planning Poker, 2002)
> 在 SDIE 中引用位置：Decomposition 讨论（task 级 `estimate` 字段，模板当前未纳入）
> 用途：对原子任务做"相对工作量"估算，支撑排期与 Gate 2 规模判断。

## 定义
- **Story Points（故事点）**：衡量"相对工作量"的抽象数值，**不是工时、不是功能大小**。
- 一个故事点同时含 Effort（工作量）+ Complexity（复杂度）+ Uncertainty（不确定性）三维度。
- **Planning Poker（计划扑克）**：基于 Wideband Delphi 的共识估算游戏，James Grenning 2002 首创、Cohn 2005 推广。
- 数值用 **Fibonacci**（1, 2, 3, 5, 8, 13…），间距大以减少虚假精度。

## 核心要素
- 相对性：只问"比基准复杂多少"，团队内部一致即可。
- 不承诺精确工时，避免估算压力。

## 在 SDIE 中的用法（建议，非强制）
- DECOMP task 可加 `estimate` 字段：Fibonacci 或 T-shirt 尺寸（S / M / L）。
- 各 task 点数求和 → Gate 2 时 Tech Lead 判断整包工作量、Implement 排期。
- 高 estimate 的 task 可能需要更长上下文 / 更多变异测试轮次。

## 权威出处
- Cohn, M. "Agile Estimating and Planning", 2005.
- Grenning, J. "Planning Poker", 2002.
