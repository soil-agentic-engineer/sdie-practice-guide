## 元信息（meta）

```yaml
id: PRD-<FEATURE>-001
title: PRD：<功能名>
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - UserStoryMap-<feature>.md
  - AC-<feature>.md
  - TASK-<feature>.yaml
last_updated: 2026-08-06
```

# PRD：<功能名>

> 红线①：业务需求与验收语义拍板归 PM/SME，Agent 仅补草稿。Gate 1：status=baseline + PM 签 + QA 复核验收可测方进 Design。

## 1. 背景与目标（Business Goal）
   - 目标用 Goal 写法，SMART；每条赋编号 `GOAL-x`（x 从 1 递增），供 §4 Impact 精确引用。
   - 单/多 Goal 均适用；多 Goal 时每条独占一行编号。

- **GOAL-1**（SMART）：在 Q3 将放弃购物车率从 X% 降到 Y%
- **GOAL-2**（SMART）：将活动上线周期从 Z 天降到 W 天

## 2. 范围（Scope）
   - in_scope: [...]
   - out_of_scope: [...]   ← PM 在 Gate 1 标注，防范围蔓延

## 3. 用户与角色（Actors）
   - 主要角色：...；次要角色：...；off-stage：合规/支付网关

## 4. 行为影响（Impacts）
   - 每条 Impact 回答"哪些角色的什么行为要变成什么样，才能促成 §1 目标"。
   - off-stage 角色（§3）同样必须列行为条目，不可省略。
   - 编号：每条 Impact 赋 `IMP-x`（x 从 1 递增）；"关联 Goal"列填 §1 的 `GOAL-x` 编号，**禁止用自然语言或"同上"引用**。
   - 一条 Impact 可关联多个 Goal（用 `GOAL-1, GOAL-2` 逗号分隔）；无对应 Goal 的 Impact 不允许存在（Gate 1 退回）。

| IMP-x | Actor（来自 §3） | 当前行为 | 目标行为（Impact） | 关联 Goal（§1 编号） |
|---|---|---|---|---|
| IMP-1 | 买家 | 库存不足时下单失败、反复重试 | 库存>0 才加购，实时看到可购数量 | GOAL-1 |
| IMP-2 | 客服 | 手工核对库存、重复解释 | 系统自动提示缺货原因，无需转人工 | GOAL-1 |
| IMP-3 | off-stage：合规网关 | 审批 T+2 天 | 审批 T+0，活动上线不等合规 | GOAL-2 |

### 4.1 Goal→Impact 校验矩阵（Gate 1 双向 trace 用）

- 行=Goal（§1），列=Actor（§3），格填 `IMP-x`；空格表示该 Goal 下该 Actor 无 Impact（允许）。
- 若某 Goal 所在行全空 → 该 Goal 无任何行为支撑，Gate 1 退回 PM 重审目标可达成性。

| Goal \ Actor | 买家 | 客服 | 合规网关 |
|---|---|---|---|
| GOAL-1 | IMP-1 | IMP-2 | — |
| GOAL-2 | — | — | IMP-3 |

## 5. 用户故事地图摘要（链接 story-map 文档）
   - 见 `UserStoryMap-<feature>.md`
   - 每条 story 需标注其支撑的 Impact（IMP-x，来自 §4），未挂 Impact 的 story 视为范围外候选（§2 复核）

## 6. 验收标准集（嵌入 acceptance_criteria）
   - 见 `AC-<feature>.md` 或下方章节
   - 每条 AC 需 trace 至 §4 中至少一个 Impact（IMP-x），保证"行为改变→验收可测"闭环

## 7. 非目标与假设（Non-goals / Assumptions）
   - ...

## 8. 优先级（MoSCoW + KANO 双维标注）

> 优先级双维（MoSCoW / KANO）的定义与"双维相互独立，勿机械交叉"约束见 `spec-guide.md` §五；本模板仅保留落地用的矩阵与标注表。

**① 映射矩阵（判定规则，仅作交叉校验参考）**

| MoSCoW \ KANO | Basic | Performance | Excitement |
|---|---|---|---|
| **Must** | MVP 硬底线，必进首版 | 核心价值，按容量排 | 罕见，若命中需 PM 复核 |
| **Should** | 同上但可协商 | 核心价值 | 差异化候选 |
| **Could** | 一般不做 | 后续 release | 亮点，放 A/B 或下迭代 |
| **Won't / Indifferent / Reverse** | — | — | 剔除或 `deferred` |

**② 双维标注（功能需求清单逐条标注）**

PRD 功能需求清单中，每个功能条目同时标注 `KANO 类型` + `MoSCoW 等级`，并附 `判断理由` 与 `所属版本`；评级由 PM=A/R 主标（不可委托 ①），Agent 仅建议不签字。

| 功能条目 (ID) | KANO 类型 | MoSCoW 等级 | 判断理由 | 所属版本 | 对应 Impact |
|---|---|---|---|---|---|
| REQ-1 库存>0 才能加购 | Basic | Must | 缺则无法下单，属用户必然预期 | MVP (v1.0.0) | IMP-1（买家即时下单） |
| REQ-2 加购后数量实时同步 | Performance | Must | 多端一致为核心体验底线 | MVP (v1.0.0) | IMP-1 |
| REQ-3 加购成功撒花动画 | Excitement | Could | 加分项，缺失不影响主流程 | v1.1.0 | 无（纯体验） |
| REQ-4 加购时弹调查问卷 | Reverse | Won't | 打断转化、有反效果，剔除 | deferred | — |

> 每个 REQ 应 trace 到 §4 中至少一个 Impact（除非标注"纯体验"）；无法 trace 的条目，PM 需在 Gate 1 复核其是否应进范围（§2）。

> 本节约不引入串联决策流程（KANO 筛选 → RICE 排序 → MoSCoW 收口）。优先级以"逐条双维标注"方式直接落表，不走三步串联。

**③ 红线（不可委托 ①）**：优先级定级属业务语义拍板，归 PM/SME；Agent 仅可"建议"标签，不能"定级签字"。Gate 1 前冻结。

---

## 版本历史

| 版本 | 日期 | 变更摘要 | 关联 ADR |
|------|------|----------|----------|
| 1.0.0 | 2026-08-08 | 初始 baseline：背景/范围/用户/验收/优先级齐备，过 Gate 1 | — |
