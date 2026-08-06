---
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
---

# PRD：<功能名>

> **RACI**：PM/PO = A/R（① 业务需求与验收语义拍板，不可委托）；SME / Tech Lead / QA = C；安全/红队、Reviewer = I。
> **Gate 1**：本 PRD 需 `status=baseline` 且经 PM 签 Gate 1、QA 复核验收可测，方可进入 Design（§6）。
> 正文结构依据 `SDIE-Spec-Guide.md` §6.1；方法论标注见 §8.2。

## 1. 背景与目标（Business Goal）
   - 目标（建议用 Impact Mapping 的 Goal 写法，SMART，权威推荐）：
     "在 Q3 将<某指标>从 X% 降到 Y%"

## 2. 范围（Scope）
   - in_scope: [...]
   - out_of_scope: [...]   ← PM 在 Gate 1 标注，防范围蔓延（§3.5.1 #5）

## 3. 用户与角色（Actors）
   - 主要角色：...；次要角色：...；off-stage：合规/支付网关

## 4. 用户故事地图摘要（链接 story-map 文档）
   - 见 `UserStoryMap-<feature>.md`

## 5. 验收标准集（嵌入 acceptance_criteria）
   - 见 `AC-<feature>.md` 或下方章节

## 6. 非目标与假设（Non-goals / Assumptions）
   - ...

## 7. 优先级（MoSCoW + KANO 双维标注，权威推荐）

> 方法论文献：MoSCoW — Dai Clegg (1994) / DSDM；KANO — Noriaki Kano (1984) "Attractive Quality and Must-Be Quality"。操作细节见 `SDIE-Spec-Guide.md` §5.x 与 §8。

**① MoSCoW（交付承诺分级）**
- `Must`：无则发布失败，必做
- `Should`：理应做，尽力安排
- `Could`：有容量再做
- `Won't`：本期不做（移入故事地图 `deferred`）

**② KANO（用户满意度贡献分级）**
- `Basic`：基础质量，没有就不满
- `Performance`：越多越好，线性提升满意度
- `Excitement`：超出预期，惊喜点
- `Indifferent`：有无无所谓
- `Reverse`：有反效果，应剔除

**③ 映射矩阵（判定规则，仅作交叉校验参考）**

| MoSCoW \ KANO | Basic | Performance | Excitement |
|---|---|---|---|
| **Must** | MVP 硬底线，必进首版 | 核心价值，按容量排 | 罕见，若命中需 PM 复核 |
| **Should** | 同上但可协商 | 核心价值 | 差异化候选 |
| **Could** | 一般不做 | 后续 release | 亮点，放 A/B 或下迭代 |
| **Won't / Indifferent / Reverse** | — | — | 剔除或 `deferred` |

> 注意：两维相互独立，勿机械交叉。`Must` 未必等于 `Basic`（如 `Must+Excitement` 罕见，需 PM 复核）；`Excitement` 不应优先于 `Basic` 底线。

**④ 双维标注（功能需求清单逐条标注）**

PRD 功能需求清单中，每个功能条目同时标注 `KANO 类型` + `MoSCoW 等级`，并附 `判断理由` 与 `所属版本`；评级由 PM=A/R 主标（不可委托 ①），Agent 仅建议不签字。

| 功能条目 (ID) | KANO 类型 | MoSCoW 等级 | 判断理由 | 所属版本 |
|---|---|---|---|---|
| REQ-1 库存>0 才能加购 | Basic | Must | 缺则无法下单，属用户必然预期 | MVP (v1.0.0) |
| REQ-2 加购后数量实时同步 | Performance | Must | 多端一致为核心体验底线 | MVP (v1.0.0) |
| REQ-3 加购成功撒花动画 | Excitement | Could | 加分项，缺失不影响主流程 | v1.1.0 |
| REQ-4 加购时弹调查问卷 | Reverse | Won't | 打断转化、有反效果，剔除 | deferred |

> `所属版本` 与 User Story Map 的 `release` 横线、`SDIE-Spec-Guide.md` §7 的 SemVer 对齐：`MVP=v1.0.0`、后续迭代=`v1.1.0`、`deferred`=剔除。

> **本节约不引入串联决策流程**（KANO 筛选 → RICE 排序 → MoSCoW 收口）。优先级以"逐条双维标注"方式直接落表，不走三步串联。

**⑤ 红线（不可委托 ①）**：优先级定级属业务语义拍板，归 PM/SME；Agent 仅可"建议"标签，不能"定级签字"。Gate 1 前冻结。
