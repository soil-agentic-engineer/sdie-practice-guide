# WBS / PMBOK — 工作分解结构

> 引用来源：PMI, A Guide to the Project Management Body of Knowledge (PMBOK Guide)；WBS 标准实践
> 在 SDIE 中引用位置：SDIE-Design-Guide §6.2、2-Design/decomposition.template.yml
> 用途：为 Task Decomposition 提供"可交付成果导向的层级分解"权威基准。

## 定义
WBS（Work Breakdown Structure）= "deliverable-oriented hierarchical decomposition of work"（面向可交付成果的层级分解）。最低层 = Work Package（可委派单一负责人的工作包）。

## 核心要素
- **100% 规则**：子项之和 = 父项 100% 范围（无遗漏、无重复）。
- 层级（树状父子）而非扁平列表。
- 配 **WBS Dictionary** 描述每个元素的边界 / 负责人（= SDIE 的人类审阅叙事层）。

## 在 SDIE 中的用法
- Decomposition 把 `TASK-*.yaml`（契约）拆为 `DECOMP-*` 原子任务，对应 WBS 的 Work Package 思想。
- 建议补"100% 规则校验"：原子任务覆盖 TASK 全部范围，无遗漏 / 重复。
- 与纯 Markdown 表格相比，层级 + 字典叙述更贴合 WBS 标准（SDIE 采用纯 YAML 单文档 + 注释承载叙事）。

## 权威出处
- Project Management Institute. "A Guide to the Project Management Body of Knowledge (PMBOK® Guide)".
- workbreakdownstructure.com（WBS 实践站）
