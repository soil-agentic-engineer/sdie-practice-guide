# KANO 模型 — 需求满意度分类

> 引用来源：Noriaki Kano et al., "Attractive Quality and Must-be Quality", 1984
> 在 SDIE 中引用位置：prd.template §7、SDIE-Spec-Guide §5.2（与 MoSCoW 双维标注）
> 用途：按"实现与否对用户满意度的影响"对需求分类，辅助优先级判断。

## 定义
KANO 将需求分为五类（经典三核心 + 两衍生）：
- **Basic（必备 / 底线）**：没有会严重不满，有了不会额外满意。
- **Performance（期望 / 线性）**：越多越好，满意度线性提升。
- **Excitement（兴奋 / 魅力）**：超出预期，有了惊喜，没有也不不满。
- Indifferent（无差异）、Reverse（反向）为衍生类。

## 核心要素
- 通过"功能具备时 / 不具备时"双问问卷定位归类。
- 时间推移会漂移：Excitement → Performance → Basic。

## 在 SDIE 中的用法
- 在 PRD 功能需求清单中，每个条目**同时标注 KANO 类型 + MoSCoW 等级 + 判断理由 + 所属版本**（用户裁定，长期有效）。
- 与 MoSCoW 相互独立、不作机械交叉映射；Basic 底线优先于 Excitement。
- 评级 RACI：PM = A/R 主标，Agent 仅建议不签字（① 不可委托）。

## 权威出处
- Kano, N., Seraku, N., Takahashi, F., Tsuji, S. (1984). "Attractive Quality and Must-be Quality".
- 用户裁定：PRD 双维标注，排除"串联决策（KANO→RICE→MoSCoW）"流程。
