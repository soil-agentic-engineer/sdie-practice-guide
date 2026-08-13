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

> **RACI**：PM/PO = A/R（① 业务需求与验收语义拍板，不可委托）；SME / Tech Lead / QA = C；安全/红队、Reviewer = I。
> **Gate 1**：本 PRD 需 `status=baseline` 且经 PM 签 Gate 1、QA 复核验收可测，方可进入 Design（§6）。
> 正文结构依据 `SDIE-Spec-Guide.md` §4.1；方法论标注见 §8.2。

## 1. 背景与目标（Business Goal）
   - 目标（建议用 Impact Mapping 的 Goal 写法，SMART，权威推荐）：
     "在 Q3 将<某指标>从 X% 降到 Y%"

## 2. 范围（Scope）
   - in_scope: [...]
   - out_of_scope: [...]   ← PM 在 Gate 1 标注，防范围蔓延（§3.5.1 #5）

## 3. 用户与角色（Actors）
   - 主要角色：...；次要角色：...；off-stage：合规/支付网关

## 4. 行为影响（Impacts）
   - 方法论：Impact Mapping 第 3 层——"角色需发生的行为改变"（Gojko Adzic，权威推荐）。
   - 写法：每条 Impact 回答"哪些角色的什么行为要变成什么样，才能促成 §1 目标"。
   - 约束：off-stage 角色（§3）同样必须列行为条目，不可省略。

| Actor（来自 §3） | 当前行为 | 目标行为（Impact） | 关联 Goal（§1） |
|---|---|---|---|
| 买家 | 库存不足时下单失败、反复重试 | 库存>0 才加购，实时看到可购数量 | 将放弃购物车率从 X% 降到 Y% |
| 客服 | 手工核对库存、重复解释 | 系统自动提示缺货原因，无需转人工 | 同上 |
| off-stage：合规网关 | 审批 T+2 天 | 审批 T+0，活动上线不等合规 | 将上线周期从 Z 天降到 W 天 |

## 5. 用户故事地图摘要（原 §4，自 1.2.0 起顺延；链接 story-map 文档）
   - 见 `UserStoryMap-<feature>.md`
   - 每条 story 需标注其支撑的 Impact（IMP-x，来自 §4），未挂 Impact 的 story 视为范围外候选（§2 复核）

## 6. 验收标准集（原 §5，自 1.2.0 起顺延；嵌入 acceptance_criteria）
   - 见 `AC-<feature>.md` 或下方章节
   - 每条 AC 需 trace 至 §4 中至少一个 Impact（IMP-x），保证"行为改变→验收可测"闭环

## 7. 非目标与假设（Non-goals / Assumptions）（原 §6，自 1.2.0 起顺延）
   - ...

## 8. 优先级（MoSCoW + KANO 双维标注，权威推荐）（原 §7，自 1.2.0 起顺延）

> 方法论文献：MoSCoW — Dai Clegg (1994) / DSDM；KANO — Noriaki Kano (1984) "Attractive Quality and Must-Be Quality"。操作细节见 `SDIE-Spec-Guide.md` §5.2 与 §8。

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

| 功能条目 (ID) | KANO 类型 | MoSCoW 等级 | 判断理由 | 所属版本 | 对应 Impact |
|---|---|---|---|---|---|
| REQ-1 库存>0 才能加购 | Basic | Must | 缺则无法下单，属用户必然预期 | MVP (v1.0.0) | IMP-1（买家即时下单） |
| REQ-2 加购后数量实时同步 | Performance | Must | 多端一致为核心体验底线 | MVP (v1.0.0) | IMP-1 |
| REQ-3 加购成功撒花动画 | Excitement | Could | 加分项，缺失不影响主流程 | v1.1.0 | 无（纯体验） |
| REQ-4 加购时弹调查问卷 | Reverse | Won't | 打断转化、有反效果，剔除 | deferred | — |

> `所属版本` 与 User Story Map 的 `release` 横线、`SDIE-Spec-Guide.md` §6.3 的 SemVer 对齐：`MVP=v1.0.0`、后续迭代=`v1.1.0`、`deferred`=剔除。

> 每个 REQ 应 trace 到 §4 中至少一个 Impact（除非标注"纯体验"）；无法 trace 到任何 Impact 的功能条目，PM 需在 Gate 1 复核其是否应进范围（§2）。

> **本节约不引入串联决策流程**（KANO 筛选 → RICE 排序 → MoSCoW 收口）。优先级以"逐条双维标注"方式直接落表，不走三步串联。

**⑤ 红线（不可委托 ①）**：优先级定级属业务语义拍板，归 PM/SME；Agent 仅可"建议"标签，不能"定级签字"。Gate 1 前冻结。

---

## 版本历史（方案 A，依据 SDIE-Spec-Guide.md §6.5）

> 不进元信息七字段（七字段不变）；版本号遵循 §6.3 SemVer。权威溯源见 `0-References/changelog.md`。

| 版本 | 日期 | 变更摘要 | 关联 ADR |
|------|------|----------|----------|
| 1.0.0 | 2026-08-08 | 初始 baseline：背景/范围/用户/验收/优先级齐备，过 Gate 1 | — |
| 1.1.0 | 2026-08-12 | MINOR：补充"游客结账"用户故事（向后兼容新增） | ADR-0012 |
| 1.2.0 | 2026-08-12 | MINOR：新增 §4 行为影响（Impacts）层，原 §4~§7 顺延为 §5~§8；功能清单加"对应 Impact"列（向后兼容） | — |
