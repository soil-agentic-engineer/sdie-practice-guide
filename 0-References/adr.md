# ADR — 架构决策记录（Architecture Decision Record）

> 引用来源：Michael Nygard, "Documenting Architecture Decisions", 2011（https://thinkrelevance.com/2011/11/15/documenting-architecture-decisions）
> 在 SDIE 中引用位置：SDIE-Spec-Guide §6.4、2-Design/adr.template.md、Design 阶段 ADR 定稿（② 不可委托）
> 用途：记录架构层面的"已做出的决策及其理由"，形成可追溯的决策史。

## 定义
ADR 是一篇简短、聚焦的文档，记录一项架构决策：背景（Context）、决策（Decision）、结果（Consequences）。Nygard 主张"把决策写下来"，因为架构决策会被遗忘、人员会流动。

## 核心要素
- **Status**：Proposed / Accepted / Deprecated / Superseded
- **Context**：面临的力（forces）与约束
- **Decision**：选择了什么
- **Consequences**：带来的正 / 负后果与权衡

## 在 SDIE 中的用法
- Design 阶段产出 ADR（如 `ADR-DESIGN-<NNN>`），由 Tech Lead 定稿（② 不可委托）。
- Decomposition 的 `related_docs` 回溯对应 ADR，保证原子任务与架构决策一致。
- ADR 状态机与文档状态机呼应：被取代的 ADR 标 Superseded。

## 权威出处
- Nygard, M. "Documenting Architecture Decisions", 2011.
- 2-Design/adr.template.md（SDIE 本地落地的模板）
