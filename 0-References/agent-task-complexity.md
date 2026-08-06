# Agent Task Complexity（面向 AI Agent 的可执行性分级）— 参考方案集

> 引用来源：Temporal "complexity cliff" Agent 能力分级（temporal.io）；SWE-bench Verified 结构难度因子（OpenAI + Princeton，Jimenez et al. 2024）；T 恤尺码法（Asana 等敏捷实践）；Parasuraman, Sheridan & Wickens 2000（自动化等级 LOA）。
> 在 SDIE 中引用位置：Decomposition 讨论（task 级 `estimated_complexity` 字段）；联动 `agent_assignable`、Implement 调度。
> 用途：为原子任务标记"Agent 干这活有多难 / 该给多少上下文 / 怎么排"，作为 Implement 阶段 Agent 调度策略与上下文预算分配信号。

## 定义
- **Agent Task Complexity（面向 Agent 的可执行性分级）**：衡量一个原子任务交由 Coding Agent 作为 R（执行者）完成时所需的**执行成本与认知难度**，而非人类交付工时（≠ Story Points / 人天）、亦非不确定性对目标的威胁（≠ Risk）。
- 在 SDIE 中，`estimated_complexity: L1–L5` 是我们自定义的序数分级标签，其**级别定义锚定外部方案的客观信号**（见下文），不凭主观感觉打分。

## 核心要素
- 与 `risk`（威胁轴）、`estimate`（工作量轴，本模板不含）**三者正交**。
- 低复杂任务可能仍 `agent_assignable:false`（涉安全/不可委托红线）；高复杂任务可能 `agent_assignable:true`（人类问责 + Agent 干），两轴独立。
- 参考方案按"所量之轴"可分四类：

| 方案 | 分级 | 量的是什么轴 | 权威性 | 出处 |
|------|------|--------------|--------|------|
| **A. SDIE 草案 L1–L5** | 5 档 | Agent 可执行性（自定，锚定 B+C） | 本仓库自定义 | — |
| **B. Temporal Agent 能力等级** | L1–L5 | **执行时长 / 失败成本 / 迭代成本**（Reflexive→Conversational→需持久执行→跨系统协调→自治） | 行业 taxonomy（非同行评审） | temporal.io/blog/building-ai-agents-that-overcome-the-complexity-cliff |
| **C. SWE-bench Verified 结构难度因子** | 非枚举，是维度 | **改文件数(1–2 vs 5+)、改行数(1–20 vs 50+)、跨子系统、领域知识、issue 清晰度、测试复杂度** | 学术（OpenAI + Princeton） | openai.com/index/introducing-swe-bench-verified；Jimenez et al. 2024 |
| **D. T 恤尺码 XS–XXL** | 5–6 档 | 相对规模（effort/complexity/uncertainty 混合） | 敏捷实践（格式借鉴） | asana.com/resources/t-shirt-sizing |
| **E. Parasuraman 10 级自动化(LOA)** | L1–L10 | **人/机职能分配（谁决策谁执行）** | 学术（人因工程） | Parasuraman, Sheridan & Wickens 2000；MIT OCW 6.804/16.400 |

## 客观信号清单（据 B + C 重定义 SDIE L1–L5）
级别定义不再依赖"感觉它挺难"，而锚定到可勾选的结构信号：

| 级别 | Agent 可执行性 | 客观信号（来自 SWE-bench 结构因子 + Temporal 执行成本） | 调度暗示 |
|------|----------------|--------------------------------------------------------|----------|
| **L1** | 机械式 | 单文件、模式清晰、AC 明确、无并发/无安全边界、改动行数≤20 | 小上下文、可并行自动 |
| **L2** | 简单 | 同模块 1–2 文件、少量边界、改动行数 20–50、有既有模式参考 | 标准上下文 |
| **L3** | 中等 | 跨模块、需对齐多服务契约、无既有模式、改文件数 2–5 | 较大上下文 + Review |
| **L4** | 复杂 | 跨子系统 / 需新算法 / 中等领域知识、改文件数≥5、测试复杂 | 最大上下文 + 人工 checkpoint |
| **L5** | 极复杂 | 无锁并发 / 架构级改动 / 强安全边界 / issue 语义模糊须人类定骨架 | 等价于 `agent_assignable:false` 场景 |

## 评分方式（建议）
1. Dev（R）起草时按上表信号勾选，初拟 `estimated_complexity`。
2. 可选增强：用 **Cyclomatic / Cognitive Complexity**（McCabe 1976；SonarSource）做**自动预标**信号，人工在 Gate 2 确认。
3. 终值在 Gate 2 由 Tech Lead（A）确认，与 `agent_assignable` / `context_scope` / `acceptance_ref` 一并复核。
4. 行内补一句依据注释（方案 A 风格），如 `estimated_complexity: L5  # 无锁并发算法，无既有模式，需人类定骨架`。

## 在 SDIE 中的用法
- `estimated_complexity` 为**可选**字段（建议非强制）：仅对需显式指导调度的 task 标注；琐事可不标。
- 驱动 Implement 阶段 Agent 调度策略（选 Agent 规格 / 并行度 / 是否再拆）与上下文预算分配（与 `context_scope` 的"范围"互补为"范围 + 预算"）。
- 与 `risk` 交叉校验：高复杂常对应加强 Review；高复杂 + `agent_assignable:false` ⟹ 人类写骨架。
- 与 `risk` 同级，均须 Dev 起草、Tech Lead Gate 2 确认；二者共同构成"难度 + 威胁"双信号输入。

## 权威出处
- Temporal Technologies. "Building AI agents that overcome the complexity cliff"（Agent 能力 L1–L5，按执行/失败/迭代成本）。 https://www.temporal.io/blog/building-ai-agents-that-overcome-the-complexity-cliff
- OpenAI + Princeton. "Introducing SWE-bench Verified"（结构难度因子：文件数、行数、跨子系统、领域知识、issue 清晰度、测试复杂度）。 https://openai.com/index/introducing-swe-bench-verified ；Jimenez et al., "SWE-bench: Can Language Models Resolve Real-world GitHub Issues?", 2024.
- Asana. "T-shirt sizing in agile"（相对规模尺码法，作格式借鉴）。 https://asana.com/resources/t-shirt-sizing
- Parasuraman, R., Sheridan, T. B., & Wickens, C. D. (2000). "A Model for Types and Levels of Human Interaction with Automation." IEEE Transactions on Systems, Man, and Cybernetics. （自动化等级 LOA，量人/机职能分配，正交轴）
- McCabe, T. J. (1976). "A Complexity Measure." （Cyclomatic Complexity，自动预标信号之一）
