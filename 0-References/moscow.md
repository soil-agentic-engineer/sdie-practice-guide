# MoSCoW — 优先级分类（DSDM）

> 引用来源：Dai Clegg, 1994, within DSDM (Dynamic Systems Development Method)；后由 Oracle 采用
> 在 SDIE 中引用位置：PRD-template §7、SDIE-Spec-Guide §5.2（与 KANO 双维标注）
> 用途：在一个时间盒（迭代 / Sprint）内，按"非做不可吗"对需求分级。

## 定义
四档：
- **Must（必须有）**：没有就不交付，≤ 60% 容量。
- **Should（应该有）**：重要但可让步。
- **Could（可以有）**：锦上添花，有时间才做。
- **Won't（这次不做）**：本次明确不做，但已考虑——给被排除需求一个去处。

## 核心要素
- 核心问题不是"重不重要"，而是"在这个时间盒里非做不可吗"。
- Must 占比须受控（≤ 60%），否则失去优先级意义。

## 在 SDIE 中的用法
- 与 KANO 双维标注于 PRD 功能需求清单，每个条目附 MoSCoW 等级 + 判断理由 + 所属版本。
- DECOMP task 的 `priority` 可**直接承接 PRD 的 MoSCoW 等级**，形成 PRD → TASK → DECOMP 的优先级传递链。
- Won't(this time) 用于防范围蔓延。

## 权威出处
- Clegg, D. "Making resource decisions for software projects", 1994 (DSDM).
- 用户裁定：双维标注、排除串联决策。
