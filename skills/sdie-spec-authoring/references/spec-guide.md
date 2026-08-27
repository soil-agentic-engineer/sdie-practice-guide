---
id: DOC-SPEC-GUIDE-001
title: SDIE Spec 阶段工作指南（产出物 / 方法论 / 字段级内容）
status: draft
phase: Spec
owner: PM/PO（产品负责人）
related_docs:
last_updated: 2026-08-12
---

# SDIE Spec 阶段工作指南

## 一 Spec 阶段与 Gate 1

- Spec 是需求与验收语义的源头与意图锚点，经 **Gate 1** 进入 Design。
- **Gate 1 准入标准**：Task Spec 完整、AGENTS.md 最新、验收可测。
- **审批人（A）**：PM；**咨询方（C）**：QA 把关验收可测。
- **不可委托项**：① 业务需求与验收语义拍板（PM/SME）。
- 架构选型（②）留到 Design 阶段；Spec 阶段 Tech Lead 仅做架构可行性初判（C），不担 A。
- 逐项检查清单见 §七。

## 二 角色与红线

### 2.1 RACI 速查

| 角色 | RACI | 本阶段定位 |
|------|:----:|-----------|
| 产品经理 PM/PO | **A/R** | 对需求与验收语义负最终责（①），Gate 1 审批；R：业务目标→可验证需求与验收标准 |
| 业务专家 SME | **C** | 把关 acceptance_criteria 语义与领域边界，防 Agent 编造伪边界（①） |
| Tech Lead / 架构负责人 | **C** | 仅做架构可行性初判；架构选型留到 Design（②） |
| 开发工程师 Task Owner | **R** | 将 PRD 编写为结构化 Task Spec；why/what/out 人类写，Agent 仅补草稿 |
| 测试架构师 / QA | **C** | 评估测试可行性与后续测试策略雏形；Gate 1 以 C 把关验收可测 |
| 安全 / 红队 | **I** | 知会需求范围，预判后续安全关注点 |
| 评审官 Reviewer | **I** | 知会 Task Spec 内容，便于 Implement 阶段审查对齐 |

### 2.2 不可委托红线

| # | 不可委托事项 | 归属角色 | 说明 |
|---|-------------|---------|------|
| ① | 业务需求与验收语义拍板 | PM / SME | Agent 仅可补草稿，不可定义领域规则或拍板需求 |
| ⑩ | Harness 维护（AGENTS.md / 校验脚本） | Dev + Tech Lead | 全阶段，Spec/Design 为主 |

## 三 执行流程（动作级递进清单）

| # | 角色 | RACI | 具体工作项 | 对应产出物 |
|---|------|:----:|-----------|-----------|
| 1 | PM/PO | **R** | SME/业务方口述→结构化需求初稿；逐条撰写 acceptance_criteria；维护 PRD | PRD、AC |
| 2 | SME | **C** | 审 acceptance_criteria 领域语义；指出伪边界；确认术语无歧义 | — |
| 3 | Tech Lead | **C** | 需求技术可行性初判；标注高风险点供 Design | — |
| 4 | QA | **C** | 评估测试可行性与风险；建议测试策略与门禁阈值草案 | — |
| 5 | PM/PO | **A** | 主持需求澄清会；标 out_of_scope 防范围蔓延；**签 Gate 1** | Gate 1 |
| 6 | Dev (Task Owner) | **R** | 基于 PRD 起草 TASK-*.yaml 骨架（why/what/out 人类写）；标 agent_hint；提交 PM 审核 | TASK |
| 7 | 安全 / 红队 | **I** | 知会范围；预判安全关注点 | — |
| 8 | Reviewer | **I** | 知会需求；记录潜在审查关注点 | — |

## 四 产出物：字段级内容（按执行顺序）

四类产出物均需在正文第 1 节「元信息」挂七字段（见 §六）。完整结构与占位符见对应模板：[prd.template.md](prd.template.md) / [user-story-map.template.md](user-story-map.template.md) / [acceptance-criteria.template.md](acceptance-criteria.template.md) / [task-spec.template.yaml](task-spec.template.yaml)；本节只列**方法规则**，结构骨架以模板为唯一真源。

### 4.1 PRD（产品需求文档）

- 结构骨架与占位符：[prd.template.md](prd.template.md)
- 方法规则：
  - `## 1 背景与目标` 用 Goal 写法（SMART），每条赋编号 `GOAL-x`（x 从 1 递增），供 §4 Impacts 精确引用。
  - `## 4 行为影响` 每条赋 `IMP-x`；"关联 Goal"列填 §1 的 `GOAL-x` 编号（禁用自然语言或"同上"）。
  - `## 2 范围` 的 `out_of_scope` 由 PM 在 Gate 1 标注，防范围蔓延。
  - Goal↔Impact 双向 trace：每条 `GOAL-x` 至少一条 `IMP-x` 支撑（Gate 1 校验，见 §七）。

### 4.2 用户故事地图（User Story Mapping）

- 结构骨架与占位符：[user-story-map.template.md](user-story-map.template.md)
- 方法规则：
  - 每条 `story` 须过 INVEST 校验；高优先置顶、低优先置底。
  - 每条 `story` 的 `impact` 须挂 PRD §4 的 `IMP-x`；未挂 `impact` 的 story 视为范围外候选。

### 4.3 acceptance_criteria（验收标准）

- 结构骨架与占位符：[acceptance-criteria.template.md](acceptance-criteria.template.md)
- 方法规则：
  - 反面示例（无量化阈值、无行为定义、"合适的时候"等伪边界）须由 SME 拦截；Agent 不得编造伪边界（不可委托①）。
  - 写法支持 BDD/Gherkin，可与 AC 互转；Example Mapping（三 amigos）收口。
  - 须具备 Given/When/Then 或正反例，QA 作为 C 确认可测（Gate 1 依据，见 §七）。

### 4.4 TASK-*.yaml（结构化任务规格）

- 结构骨架与占位符：[task-spec.template.yaml](task-spec.template.yaml)
- 方法规则：
  - 执行步骤：Dev 起草（R）→ PM 审核 + Gate 1 签批（A）。
  - **红线**：`why` / `what` / `out` 三字段必须人类手写，Agent 仅可补 `agent_hint` 等草稿（不可委托①）。
  - `status` 不得由 Agent 直接置 `baseline`（需 Gate 1 通过）。

### 4.5 REQ → task-spec 判定规则（三层过滤，全过才建）

| 层 | 过滤依据 | 通过条件（做） | 不通过（不做） |
|----|---------|--------------|---------------|
| ① 范围 | PRD §2 in_scope / out_of_scope | REQ ∈ in_scope | REQ ∈ out_of_scope → 不建 |
| ② 双维优先级 | PRD §8 KANO + MoSCoW | MoSCoW∈{Must,Should,Could} 且 KANO∉{Reverse,Indifferent} | MoSCoW=Won't 或 KANO∈{Reverse,Indifferent} → 转 deferred / 剔除 |
| ③ 版本归属 | PRD §8 所属版本 + 故事地图 release | 所属版本 = 当前目标 release | 后续 release 或 deferred → 本期不建 |

- 执行：Dev (Task Owner) = R 基于 PRD 起草 TASK-*.yaml，why/what/out 人类手写（红线①），related_docs 回指 PRD + AC。
- 定夺：PM/PO = A/R，业务需求与验收语义拍板不可委托①，Agent 仅建议；Gate 1 前冻结。
- 确认：TASK 须 status=baseline + PM 签 Gate 1 才正式确认，进 Design。
- 粒度：按 TASK-<FEATURE>-NNN 编号（feature 级）；绑定键始终是 related_docs → PRD-<feature>.md。

## 五 优先级标注（MoSCoW + KANO 双维）

- 落点：PRD §8；联动用户故事地图 release 决定 MVP 边界。
- RACI / 红线①：PM=A/R 主导定级，SME/Tech Lead/QA=C 评审；Agent 仅可建议标签、不能定级签字。Gate 1 前冻结。
- 定义：
  - MoSCoW：Must（无则发布失败）/ Should（理应，尽力）/ Could（有空做）/ Won't（本期不做）。
  - KANO：Basic（基础质量）/ Performance（越多越好）/ Excitement（超出预期）/ Indifferent（无所谓）/ Reverse（有反效果）。
- 双维相互独立，勿机械交叉。Must 未必等于 Basic；Excitement 不应优先于 Basic 底线。
- PRD 功能清单逐条标注 KANO 类型 + MoSCoW 等级 + 判断理由 + 所属版本。

## 六 元信息与状态机

### 6.1 元信息七字段（所有 Spec 材料第 1 节挂此元数据）

| 字段 | 含义 | 示例 |
|------|------|------|
| `id` | 文档唯一标识 | DOC-SPEC-GUIDE-001 / TASK-CART-001 |
| `title` | 标题 | SDIE Spec 阶段工作指南 |
| `status` | 生命周期状态（见 6.2） | draft |
| `phase` | 所属 SDIE 阶段 | Spec |
| `owner` | 责任人类角色 | PM/PO |
| `related_docs` | 关联文档（双向链接） | [PRD-checkout, AC-cart] |
| `last_updated` | 最后更新日期 | 2026-08-05 |

### 6.2 文档状态机（status 字段取值与流转）

```
draft ──(需求澄清会 + 评审通过)──▶ review
review ──(PM 签 Gate 1)──────────▶ baseline（基线化，进入 Design 的准入）
baseline ──(发现需修改)───────────▶ change（开变更，生成新版本号）
change ──(变更评审通过)───────────▶ baseline（新基线）
baseline ──(被新方案替代)─────────▶ superseded by <new-id>
```

基线化（baseline）是硬门槛：只有 status=baseline 的 Spec 才能过 Gate 1 进入 Design。

### 6.3 基线化变更流程（Spec 迭代闭环）

1. 对 baseline Spec 的修改请求 → 开 change 状态副本，记录变更动机（可挂 ADR）。
2. 按 §三 递进流程重新走：PM 改稿（R）→ SME/Tech Lead/QA 审（C）→ PM 定稿（A）。
3. 版本号按 MAJOR.MINOR.PATCH 递增（不兼容变更升 MAJOR，向后兼容新增升 MINOR，修正升 PATCH）；last_updated 刷新；related_docs 互链新旧版本。
4. 重新签 Gate 1（PM 审批，QA 复核验收可测）→ 新 baseline。
5. 旧版本置 superseded by <new-id>，保留考古记录，不删除。

版本历史段落（必填）：每一篇基线化的阶段产出物，须在本正文末尾维护 `## 版本历史` 段落，格式 `| 版本 | 日期 | 变更摘要 | 关联 ADR（可选）|`，与状态机、版本号协同。本段落不进元信息七字段。

## 七 Gate 1 准入检查清单

| # | 检查项 | 对应产出物 / 依据 | 不可委托红线 |
|---|--------|------------------|--------------|
| 1 | Task Spec 完整：四类齐备，均挂元信息七字段；TASK-*.yaml 的 why/what/out 为人类手写 | 1-Spec/* 模板；§四 | ① |
| 2 | AGENTS.md 最新：执行边界承载与最新 RACI 同步 | AGENTS.md；§2.1 | ⑩ |
| 3 | 验收可测：acceptance_criteria 具备 Given/When/Then 或正反例，QA 作为 C 确认可测 | acceptance_criteria；§4.3 | ① |
| 4 | 优先级已冻结：PRD 功能清单逐条双维标注，Gate 1 前冻结，无串联决策 | prd.template.md §8；§五 | ① |
| 5 | Goal↔Impact 双向 trace：每条 GOAL-x 至少一条 IMP-x 支撑；每条 IMP-x 的"关联 Goal"列填 GOAL-x 编号 | prd.template.md §1/§4/§4.1 | ① |
| 6 | 基线化完成：Spec 包 status=baseline（非 draft/review） | §6.2 状态机 | 仅 baseline 才能过 Gate 1 |

放行动作：PM 在 #1–#5 全部通过后，签 Gate 1 Approved，Spec 包方可进入 Design。任一不通过即退回对应 R 修正，不得带伤过门。
