# Spec 阶段方法集（需求 / 规格方法论）

> 引用来源：多名原作者（见各条）；在 SDIE 中引用位置：SDIE-Spec-Guide §5「方法论映射」
> 用途：Spec 阶段把模糊业务意图收敛为可验证验收标准时可借用的权威方法论（空间已定义之外、权威推荐补足）。

## 各方法速查

### Impact Mapping（影响地图）
- 来源：Gojko Adzic, "Impact Mapping", 2012。
- 定义：从**目标 → 角色 → 影响 → 交付物**四层推导，避免功能堆积。
- SDIE 用法：PRD 目标对齐，连接业务目标与用户故事。

### User Story Mapping（用户故事地图）
- 来源：Jeff Patton, "User Story Mapping", 2014。
- 定义：以"骨架（主干活动）+ 走查（用户旅程）+ 切片（发布计划）"组织故事，兼顾全景与优先级。
- SDIE 用法：`1-Spec/UserStoryMap-template.md` 承载。

### INVEST（好故事准则）
- 来源：Bill Wake, 2003（"INVEST in Good Stories"）。
- 定义：Independent / Negotiable / Valuable / Estimable / Small / Testable。
- SDIE 用法：PRD 用户故事质量门槛。

### BDD / Gherkin（行为驱动 + 场景语法）
- 来源：Dan North, 2003+（BDD）；Gherkin = Cucumber 场景语法（Given / When / Then）。
- 定义：以"示例即规格"描述行为，Gherkin 三段式表达验收场景。
- SDIE 用法：acceptance_criteria 推荐用 Gherkin 表达（SDIE-Spec-Guide §4.3）。

### Specification by Example（实例化需求）
- 来源：Gojko Adzic, "Specification by Example", 2011。
- 定义：用具体示例代替模糊需求，示例即活文档。
- SDIE 用法：验收标准与测试设计衔接。

### Event Storming（事件风暴）
- 来源：Alberto Brandolini, 2013+。
- 定义：以"领域事件"为主线、便签协作的轻量建模工作坊。
- SDIE 用法：复杂业务梳理、PRD 前探索。

## 权威出处
- Adzic, G. "Impact Mapping" (2012), "Specification by Example" (2011).
- Patton, J. "User Story Mapping" (2014).
- Wake, B. "INVEST" (2003).
- North, D. BDD (2003+).
- Brandolini, A. Event Storming (2013+).
