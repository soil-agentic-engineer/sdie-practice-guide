---
id: DOC-SPEC-GUIDE-001
title: SDIE Spec 阶段工作指南（工作项 / 产出物 / 方法论 / 字段级内容 / 版本化管理）
status: draft
phase: Spec
owner: PM/PO（产品负责人）
related_docs:
  - SDIE-RACI-Matrix.md
  - SDIE-Analysis.md
last_updated: 2026-08-12
---

# SDIE Spec 阶段工作指南

> 本文是 SDIE 工程方法在 **Spec（规格）阶段** 的操作手册，系统回答五个问题：
> ① Spec 应执行哪些工作？② 分别应输出哪些材料？③ 可用什么方法论产出这些材料？
> ④ 材料内容具体有哪些（字段级）？⑤ 如何版本化、迭代与管理？
>
> **权威来源约定（用户硬约束）**：本文所有 SDIE 事实仅取自当前工作空间
> `SDIE-RACI-Matrix.md`（7 人类 / 12 Agent 治理版，唯一权威明细）与 `SDIE-Analysis.md`。
> 方法论与外部实践通过 WebSearch 取得权威定义并标注出处。

> **五问速览**：① 工作 → [§3](#sec-3) ② 材料 → [§4](#sec-4) ③ 方法论 → [§5](#sec-5) ④ 字段级内容 → [§4](#sec-4) ⑤ 版本化 → [§6](#sec-6)

---

## 目录

- [一 Spec 阶段定位](#sec-1)
  - [1.1 SDIE 框架与 Spec 定位](#sec-1-1)
  - [1.2 Gate 1 准入门禁概览](#sec-1-2)
- [二 角色与 RACI](#sec-2)
  - [2.1 RACI 速查](#sec-2-1)
  - [2.2 不可委托红线](#sec-2-2)
- [三 Spec 执行流程（递进工作清单）](#sec-3)
  - [3.1 执行总览](#sec-3-1)
  - [3.2 动作级递进清单](#sec-3-2)
- [四 产出物：字段级内容（按执行顺序）](#sec-4)
  - [4.1 PRD（产品需求文档）](#sec-4-1)
  - [4.2 用户故事地图（User Story Mapping）](#sec-4-2)
  - [4.3 acceptance_criteria（验收标准）](#sec-4-3)
  - [4.4 TASK-*.yaml（结构化任务规格）](#sec-4-4)
  - [4.5 REQ → task-spec 判定规则](#sec-4-5)
- [五 方法论映射](#sec-5)
  - [5.1 方法论总表（空间已定义 vs 权威推荐）](#sec-5-1)
  - [5.2 优先级标注操作手册（MoSCoW + KANO）](#sec-5-2)
- [六 版本化 / 迭代 / 变更管理](#sec-6)
  - [6.1 元信息七字段](#sec-6-1)
  - [6.2 文档状态机](#sec-6-2)
  - [6.3 SemVer 版本号](#sec-6-3)
  - [6.4 ADR 决策记录](#sec-6-4)
  - [6.5 基线化变更流程](#sec-6-5)
  - [6.6 Git 约定](#sec-6-6)
- [七 Gate 1 准入检查清单](#sec-7)
- [八 引用与权威来源](#sec-8)
- [九 五问一句话总结](#sec-9)

---

<a id="sec-1"></a>
## 一 Spec 阶段定位

<a id="sec-1-1"></a>
### 1.1 SDIE 框架与 Spec 定位

SDIE = **S**pec / **D**esign / **I**mplement / **E**valuation，是面向 AI Coding 的治理框架。
其关键改造（见 `SDIE-RACI-Matrix.md` §1）：**A（问责 / 批准权）永远由人类承担；AI Agent 仅作为 R（执行者）**——
因为当代码可被秒级重写时，真正有价值的是"什么是对的"这一判断，必须由人做出。

四阶段中，Spec 是**源头与意图锚点**：它把模糊的业务诉求收敛为结构化、可验证、可追责的需求与验收语义，
并通过 **Gate 1** 进入 Design 阶段。

<a id="sec-1-2"></a>
### 1.2 Gate 1 准入门禁概览

Gate 1 是 Spec → Design 的准入门禁。**准入标准**：Task Spec 完整、AGENTS.md 最新、验收可测。
**人类审批人（A）**：**PM**（产品经理 / 产品负责人）。**咨询方（C）**：**QA**——把关"验收可测"（acceptance-testability）。
**不可委托项**：① 业务需求与验收语义拍板（PM/SME）。

> 关键纪律：Gate 1 的 A 是 PM，不是 Tech Lead。架构选型（②）留到 Design 阶段，Spec 阶段
> Tech Lead 仅做**架构可行性初判**（C），不担 A（见 `SDIE-RACI-Matrix.md` §3.1 / §3.5.1）。

> Gate 1 的**逐项检查清单**见 [§7](#sec-7)，在执行完 [§3](#sec-3)（工作流程）与 [§4](#sec-4)（产出物）后对照核对。

---

<a id="sec-2"></a>
## 二 角色与 RACI

<a id="sec-2-1"></a>
### 2.1 RACI 速查

> 以下 RACI 引自 `SDIE-RACI-Matrix.md` §3.1（责任级）与 §3.5.1（动作级）。

| 角色 | RACI | 本阶段定位 |
|------|:----:|-----------|
| 产品经理 PM/PO | **A/R** | 对需求与验收语义负最终责（①），Gate 1 审批；R：将业务目标转化为可验证需求与验收标准 |
| 业务专家 SME | **C** | 把关 `acceptance_criteria` 的语义与领域边界，防止 Agent 编造伪边界（①） |
| Tech Lead / 架构负责人 | **C** | 仅做架构可行性初判；架构选型留到 Design（②） |
| 开发工程师 Task Owner | **R** | 将 PRD 编写为结构化 Task Spec；"为什么"由人类写，Agent 仅补草稿；产出 `TASK-*.yaml` |
| 测试架构师 / QA | **C** | 被咨询测试可行性与后续测试策略雏形；Gate 1 以 C 把关验收可测 |
| 安全 / 红队 | **I** | 知会需求范围，预判后续安全关注点 |
| 评审官 Reviewer | **I** | 知会 Task Spec 内容，便于 Implement 阶段审查对齐 |

> Agent 侧：Spec Agent 在 Spec 阶段为 **● 主导执行**（R 侧），产出结构化 Task Spec 草稿，
> 落位 `docs/specs/task-specs/*.yaml`；但所有产出需人类（PM/Dev）复核（见 `SDIE-RACI-Matrix.md` §4）。

<a id="sec-2-2"></a>
### 2.2 不可委托红线

> 引自 `SDIE-RACI-Matrix.md` §5 ①。

**核心不可委托红线**：业务需求与验收语义拍板必须由 PM / SME 亲自完成，
Agent 不得定义领域规则或拍板需求（见 `SDIE-RACI-Matrix.md` §2.1 角色说明）。

Spec 阶段涉及的不可委托项：

| # | 不可委托事项 | 归属角色 | 说明 |
|---|-------------|---------|------|
| ① | 业务需求与验收语义拍板 | PM / SME | Agent 仅可补草稿，不可定义领域规则或拍板需求 |
| ⑩ | Harness 维护（AGENTS.md / 校验脚本） | Dev + Tech Lead | 全阶段，Spec/Design 为主 |

---

<a id="sec-3"></a>
## 三 Spec 执行流程（递进工作清单）

<a id="sec-3-1"></a>
### 3.1 执行总览

Spec 阶段的执行遵循**递进顺序**：作者起草 → 协作咨询 → 问责定稿 → 知会。

```
PM/PO 起草需求与验收标准（R）
        │
        ▼
SME / Tech Lead / QA 评审语义与可行性（C）
        │
        ▼
PM/PO 定稿并签 Gate 1（A）
        │
        ▼
Dev 起草 TASK-*.yaml（R）
        │
        ▼
安全 / Reviewer 知会（I）
```

`acceptance_criteria` 先由 PM/PO **撰写（R）**，再交 SME / Tech Lead / QA 评审（C），
最终由 PM/PO **定稿并签 Gate 1（A）**。

> 每个步骤应产出什么材料、字段级内容长什么样，见 [§4](#sec-4)（按执行顺序排列）。
> 可用什么方法论辅助产出，见 [§5](#sec-5)。

<a id="sec-3-2"></a>
### 3.2 动作级递进清单

> 引自 `SDIE-RACI-Matrix.md` §3.5.1。行序＝本阶段**递进执行顺序**。

| # | 角色 | RACI | 具体工作项 | 对应产出物 |
|---|------|:----:|-----------|-----------|
| 1 | PM/PO | **R** | 将 SME / 业务方口述整理为结构化需求初稿；逐条撰写 `acceptance_criteria`（用可验证语言定义 done）；维护 PRD 文档与版本 | [§4.1 PRD](#sec-4-1)、[§4.3 AC](#sec-4-3) |
| 2 | SME | **C** | 逐条审 `acceptance_criteria` 领域语义；指出 Agent 易编造的伪边界条件；确认术语定义无歧义 | — |
| 3 | Tech Lead | **C** | 对需求做技术可行性初判；标注高风险技术点供 Design 阶段深入 | — |
| 4 | QA | **C** | 评估测试可行性与风险；初步建议测试策略与门禁阈值草案 | — |
| 5 | PM/PO | **A** | 主持需求澄清会，对齐业务目标与边界；标注 `out_of_scope` 防范围蔓延；**签 Gate 1**（需求评审通过）—— 对定稿的 `acceptance_criteria` 负最终责 | [§7 Gate 1 清单](#sec-7) |
| 6 | Dev (Task Owner) | **R** | 基于 PRD 起草 `TASK-*.yaml` 骨架（why/what/out 必须人类写）；标注 `agent_hint` 与上下文来源；提交 Spec Agent 草稿供 PM 审核 | [§4.4 TASK-*.yaml](#sec-4-4)、[§4.5 REQ 判定](#sec-4-5) |
| 7 | 安全 / 红队 | **I** | 知会范围；预判安全关注点（认证 / 数据 / 合规） | — |
| 8 | Reviewer | **I** | 知会需求；记录潜在审查关注点，便于 Implement 阶段对齐 | — |

---

<a id="sec-4"></a>
## 四 产出物：字段级内容（按执行顺序）

> **模板格式选型原则（Markdown vs YAML）**：Spec 产出物遵循**全局原则**（详见 `SDIE-Analysis.md` §8.1）——按「消费者类型」决定载体格式：人类契约型（PRD、用户故事地图、`acceptance_criteria`）用 **Markdown、第 1 节「元信息」七字段**；机器规格型（`TASK-*.yaml`）用 **YAML、`meta:` 块下挂七字段，其余业务字段归入分组键（如 `spec:`）**。
> 核心：**纯 YAML 仅限机器消费型规格，不应用于人类契约型文档**（PRD 转纯 YAML 收益低、牺牲人类对齐价值）。YAML 模板的七字段统一挂在 `meta:` 块下，其余业务字段归入语义分组键（如 `spec:`）。

Spec 阶段的产出物共四类，均需在正文第 1 节「元信息」挂七字段（见 [§6.1](#sec-6-1)）。

> 四类材料不是平铺的"四份独立文件"，而是**同一意图的不同抽象层**——PRD 是业务层、
> 故事地图是用户旅程层、`acceptance_criteria` 是验证层、`TASK-*.yaml` 是执行层。
> 它们通过元信息的 `related_docs` 互相链接，形成可追溯的 Spec 包。

**产出物总览**（引自 `SDIE-RACI-Matrix.md` §3.1 / §3.5.1 / §4）：

| 材料 | 主要作者（R） | 问责者（A） | 关键读者（C/I） | 落位（参考） |
|------|--------------|------------|----------------|-------------|
| **PRD**（产品需求文档） | PM/PO（R） | PM/PO（A，①） | SME(C)、Tech Lead(C)、QA(C) | `docs/specs/` |
| **用户故事地图 User Story Map** | PM/PO + Dev（R） | PM/PO（A） | SME(C)、Tech Lead(C) | `docs/specs/story-map/` |
| **`acceptance_criteria`**（验收标准集） | PM/PO（R，逐条撰写） | PM/PO（A，①） | SME(C 语义)、QA(C 可测) | 内嵌于 PRD / TASK-SPEC |
| **`TASK-*.yaml`**（结构化任务规格） | Dev (Task Owner)（R） | PM/PO（A，Gate 1） | Tech Lead(C)、QA(C)、Reviewer(I) | `docs/specs/task-specs/*.yaml` |

以下按 [§3.2](#sec-3-2) 执行顺序展开各材料的字段级内容。

<a id="sec-4-1"></a>
### 4.1 PRD（产品需求文档）

> **执行步骤**：§3.2 #1（PM/PO 起草，R）→ #5（PM/PO 定稿，A）

`元信息` 七字段见 [§6.1](#sec-6-1)。正文建议结构：

```
# PRD：<功能名>
## 1. 背景与目标（Business Goal）
   - 目标用 Impact Mapping 的 Goal 写法，SMART；每条赋编号 GOAL-x，供 §4 Impact 引用
   - GOAL-1（SMART）："在 Q3 将购物车放弃率从 68% 降到 55%"
   - 多 Goal 时每条独占一行编号
## 2. 范围（Scope）
   - in_scope: [...]
   - out_of_scope: [...]   ← PM 在 Gate 1 标注，防范围蔓延（§3.5.1 #5）
## 3. 用户与角色（Actors）
   - 主要角色：...；次要角色：...； off-stage：合规/支付网关
## 4. 行为影响（Impacts）    ← Impact Mapping 第 3 层
   - 每条赋 IMP-x；"关联 Goal"列填 §1 的 GOAL-x 编号（禁用自然语言或"同上"）
   - §4.1 Goal→Impact 校验矩阵：Gate 1 双向 trace（每 Goal 至少 1 条 Impact，每 Impact 可反查 Goal）
## 5. 用户故事地图摘要（链接 story-map 文档）
## 6. 验收标准集（嵌入 acceptance_criteria，见 §4.3）
## 7. 非目标与假设（Non-goals / Assumptions）
## 8. 优先级（MoSCoW + KANO 标注）
```

<a id="sec-4-2"></a>
### 4.2 用户故事地图（User Story Mapping）

> **执行步骤**：§3.2 #1（PM/PO + Dev 起草，R）→ #5（PM/PO 定稿，A）

| 层级 | 字段 | 说明 |
|------|------|------|
| Backbone | `actor` | 用户视角（来自 PRD §3 的 Actors），地图围绕该用户旅程构建 |
| | `activity` | 主干活动（左→右时序），如"浏览→加购→结算→管理订单" |
| | `step` | 活动下的用户任务，如"加购"下的"查看加购按钮/点击加购" |
| Details | `story` | 实现细节，高优先置顶、低优先置底；每条 story 标注挂接的 backbone 步骤，并过 INVEST 校验 |
| | `impact` | 支撑的 PRD §4 Impact（IMP-x）；未挂 impact 的 story 视为范围外候选（见 PRD §2） |
| Release slices | `release` | 横向分割线，MVP 在顶部，未来增强在底部 |
| Out-of-scope | `deferred` | 明确"暂不做"的项，单独区放置 |

<a id="sec-4-3"></a>
### 4.3 acceptance_criteria（验收标准）

> **执行步骤**：§3.2 #1（PM/PO 逐条撰写，R）→ #2-#4（SME/Tech Lead/QA 评审，C）→ #5（PM/PO 定稿，A）

**正面示例（可测、具体、无歧义）：**
```
AC-1: 当用户在商品详情页点击"加入购物车"且库存>0 时，
      系统应在 500ms 内将商品写入购物车，并展示 toast"已加入购物车"。
AC-2: 当库存=0 时，按钮置灰并显示"已售罄"，点击不触发任何写操作。
```

**反面示例（不可测 / 伪边界，Agent 易编造，SME 须拦截）：**
```
✗ "系统应快速响应用户操作。"        ← 无量化阈值，不可测
✗ "购物车应正常工作。"             ← 无行为定义
✗ "按钮应在合适的时候禁用。"       ← "合适的时候"是伪边界，需 SME 明确
```

**BDD / Gherkin 写法（权威推荐，可与 AC 互转）：**
```gherkin
Feature: 加入购物车
  Scenario: 库存充足时加入成功
    Given 商品 P 当前库存为 5
    When  用户在商品详情页点击"加入购物车"
    Then  购物车中出现 1 件 P
    And   页面展示 toast "已加入购物车"
```

**Example Mapping 收口（权威推荐，25 分钟 / 三 amigos）：**
- 黄卡：故事"作为买家，我能把商品加入购物车"
- 蓝卡（规则）：库存>0 才可加；加购后数量+1
- 绿卡（例子）：库存 5→加购→库存不变、购物车+1
- 红卡（问题）：超卖边界（并发加购）何时处理？→ 记录为待澄清，不阻塞主流程

<a id="sec-4-4"></a>
### 4.4 TASK-*.yaml（结构化任务规格）

> **执行步骤**：§3.2 #6（Dev 起草，R）→ #5（PM 审核 + Gate 1 签批，A）
>
> **红线**：`why` / `what` / `out` 三个字段**必须人类手写**，Agent 仅可补 `agent_hint` 等草稿
>（见 `SDIE-RACI-Matrix.md` §3.5.1 #6 / §4 Spec Agent 落位）。

```yaml
meta:
  id: TASK-CART-001
  title: 购物车加入商品接口
  status: draft            # draft → review → baseline → change → superseded
  phase: Spec
  owner: dev-zhang         # Task Owner（R），起草人，非审批人（审批人见 approval:）
  related_docs:
    - PRD-checkout-2026-08.md
    - AC-cart.md
  last_updated: 2026-08-05
  approval:
    approved_by: pm-li            # PM/PO（A）签 Gate 1 Approved 时填写；Harness 校验非空才放行
    approved_at: 2026-08-05
    
spec:
  # ---- 契约区：人类强制手写核心三字段（红线①）----
  why: 购物车放弃率高，需在详情页提供一键加购以降低流失   # 业务动机，人类写
  what: 提供 POST /cart/add 接口，入参 sku+qty，出参购物车快照 # 做什么，人类写
  out: 加购成功返回 200 + 购物车快照；库存不足返回 409      # 期望产出/验收，人类写
  # ---- 追溯区：§4.5 三层过滤的冻结产物（Gate 1 前由 PM 冻结，Agent 仅起草）----
  req_ref: [REQ-1, REQ-2]       # 本任务聚合的 PRD REQ 编号
  acceptance_ref: [AC-1, AC-2]  # 覆盖的 AC 编号；下游 DECOMP acceptance_ref 的源头，禁止自创（①）
  priority:                     # PRD §7 双维评级快照（三层过滤第②层）
    kano: Basic
    moscow: Must
  target_release: MVP (v1.0.0)  # 所属版本（第③层）；与故事地图 release 横线对齐
  # ---- Agent 草稿区：可补草稿，需人类审核 ----
  agent_hint: 参考现有 order 服务的事务写法；注意并发库存校验
  context_sources:
    - src/checkout/CartService.java
    - docs/design/cart-ADR-003.md
```

<a id="sec-4-5"></a>
### 4.5 REQ → task-spec 判定规则

> 判定某个 REQ 是否需要执行 task-spec（即新建 `TASK-*.yaml`），由 **三层过滤** 组成，全过才执行。
> 依据：`1-Spec/prd.template.md` §2 范围 / §7 优先级；本文 [§4.4](#sec-4-4)；`SDIE-RACI-Matrix.md` ①。

**判定表（三层全过 → 建 task-spec）**

| 层 | 过滤依据（PRD 位置） | 通过条件（做） | 不通过（不做） |
|----|---------------------|--------------|---------------|
| ① 范围 | §2 `in_scope` / `out_of_scope` | REQ ∈ `in_scope` | REQ ∈ `out_of_scope` → 不建 |
| ② 双维优先级 | §7 `KANO 类型` + `MoSCoW 等级` | MoSCoW∈{Must, Should, Could} 且 KANO∉{Reverse, Indifferent} | MoSCoW=Won't，或 KANO∈{Reverse, Indifferent} → 不建，转 `deferred` / 剔除 |
| ③ 版本归属 | §7 `所属版本` + 用户故事地图 `release` slices | 所属版本 = 当前目标 release（如 MVP v1.0.0） | 所属版本 = 后续 release 或 `deferred` → 本期不建，延至对应 release |

**落到 task-spec（三层全过之后）**
- **执行**：Dev (Task Owner) = R 基于 PRD 起草 `TASK-*.yaml`，`why/what/out` 人类手写（红线①），`related_docs` 回指 `PRD-<feature>.md` + `AC-<feature>.md`（见 [§4.4](#sec-4-4)）。
- **定夺**：PM/PO = A/R，业务需求与验收语义拍板 **不可委托①**，Agent 仅建议；判定在 **Gate 1 前冻结**。
- **确认**：`TASK-*.yaml` 须 `status=baseline` + **PM 签 Gate 1** 才正式确认，进 Design（见 [§4.4](#sec-4-4) / [§6.2](#sec-6-2)）。
- **粒度**：`TASK-*.yaml` 按 `TASK-<FEATURE>-NNN` 编号（feature 级），一个 feature 的多个"做" REQ 聚为一条或几条 TASK；后续 Design 阶段再 1:N 拆 `DECOMP-*`。**绑定键始终是 `related_docs → PRD-<feature>.md`**。

**判定流程（一句话）**：REQ 在 `in_scope` 且 §7 双维标为"做"且所属版本=当前 release → 由 Dev 起草、PM 在 Gate 1 冻结确认 → 建 `TASK-*.yaml`；否则不建（进 `out_of_scope` 或 `deferred`）。

---

<a id="sec-5"></a>
## 五 方法论映射

<a id="sec-5-1"></a>
### 5.1 方法论总表（空间已定义 vs 权威推荐）

> 标记说明：**【空间已定义】**＝SDIE 工作空间内已规定；**【权威推荐】**＝经 WebSearch 取得的外部权威实践，作为补充方法论。

| 产出物 / 工作 | 推荐方法论 | 来源性质 | 权威出处 |
|---------------|-----------|----------|----------|
| PRD 业务目标与范围 | **Impact Mapping**（Goal→Actors→Impacts→Deliverables） | 【权威推荐】 | Gojko Adzic，impactmapping.org |
| 用户故事地图 | **User Story Mapping**（backbone/activities → steps → details → release slices） | 【权威推荐】 | Jeff Patton，jpattonassociates.com |
| 单条用户故事质量校验 | **INVEST**（Independent/Negotiable/Valuable/Estimable/Small/Testable） | 【权威推荐】 | Bill Wake 2003，xp123.com |
| `acceptance_criteria` 格式 | **BDD / Given-When-Then / Gherkin**；辅以 **Example Mapping**（黄故事/蓝规则/绿例子/红问题，25 分钟） | 【空间已定义】格式 + 【权威推荐】写法 | Dan North 2006；Cucumber，cucumber.io |
| 情境化需求表达 | **Job Stories**（When [situation], I want [motivation], so I can [outcome]） | 【权威推荐】 | Paul Adams & Alan Klement @ Intercom 2013 |
| 领域复杂性发现 | **Event Storming**（domain events / commands / actors / aggregates / hotspots） | 【权威推荐】 | Alberto Brandolini，eventstorming.com |
| 优先级排序 | **MoSCoW**（Must/Should/Could/Won't）+ **KANO**（Basic/Performance/Excitement/Indifferent/Reverse） | 【权威推荐】 | Dai Clegg 1994 / DSDM；Noriaki Kano 1984 |
| `TASK-*.yaml` 的 why/what/out | 【空间已定义】why/what/out 为人类强制手写字段 | 【空间已定义】 | `SDIE-RACI-Matrix.md` §3.5.1 / §4 |
| 验收"可测性"把关 | 【空间已定义】QA 在 Gate 1 作为 C 把关 acceptance-testability | 【空间已定义】 | `SDIE-RACI-Matrix.md` §6 Gate 1 |
| 版本化 / 迭代 / 变更 | **SemVer 2.0.0** + **ADR**（Architecture Decision Record） | 【权威推荐】 | semver.org；Michael Nygard 2011 |

**原则**：空间已定义的（元信息七字段、why/what/out 人类手写、Gate 1 验收可测把关、不可委托 ①）必须执行；
缺口的方法论以权威推荐形式补足，团队可裁剪但需记录在 ADR 中。

<a id="sec-5-2"></a>
### 5.2 优先级标注操作手册（MoSCoW + KANO）

**落点**：Spec 阶段、PRD 的「§7 优先级」一节（见 `1-Spec/prd.template.md`）；并联动 User Story Map 的 `release` slices 决定 MVP 边界。

**RACI / 不可委托 ①**：PM=A/R 主导定级（业务价值裁决），SME/Tech Lead/QA=C 评审可行性与可测性；Agent 仅可建议标签、不能定级签字（不可委托 ① 业务语义拍板归 PM/SME）。Gate 1 前冻结。

**定义**
- MoSCoW（Dai Clegg 1994 / DSDM）：Must（无则发布失败）/ Should（理应，尽力）/ Could（有空做）/ Won't（本期不做）。
- KANO（Noriaki Kano 1984）：Basic（基础质量）/ Performance（越多越好）/ Excitement（超出预期）/ Indifferent（无所谓）/ Reverse（有反效果）。

**组合判定矩阵（判定规则，仅作交叉校验参考）**

| MoSCoW \ KANO | Basic | Performance | Excitement |
|---|---|---|---|
| **Must** | MVP 硬底线，必进首版 | 核心价值，按容量排 | 罕见，若命中需 PM 复核 |
| **Should** | 同上但可协商 | 核心价值 | 差异化候选 |
| **Could** | 一般不做 | 后续 release | 亮点，放 A/B 或下迭代 |
| **Won't / Indifferent / Reverse** | — | — | 剔除或 `deferred` |

> 注意：两维相互独立，勿机械交叉。`Must` 未必等于 `Basic`；`Excitement` 不应优先于 `Basic` 底线。

**双维标注（功能需求清单逐条标注）**：PRD 功能需求清单中，每个功能条目同时标注 `KANO 类型` + `MoSCoW 等级`，并附 `判断理由` 与 `所属版本`（版本与 User Story Map `release` 横线、[§6.3](#sec-6-3) SemVer 对齐）。

**示例（购物车加购）**

| 功能条目 (ID) | KANO 类型 | MoSCoW 等级 | 判断理由 | 所属版本 |
|---|---|---|---|---|
| REQ-1 库存>0 才能加购 | Basic | Must | 缺则无法下单，属用户必然预期 | MVP (v1.0.0) |
| REQ-2 加购后数量实时同步 | Performance | Must | 多端一致为核心体验底线 | MVP (v1.0.0) |
| REQ-3 加购成功撒花动画 | Excitement | Could | 加分项，缺失不影响主流程 | v1.1.0 |
| REQ-4 加购时弹调查问卷 | Reverse | Won't | 打断转化、有反效果，剔除 | deferred |

> **本手册不引入串联决策流程**（KANO 筛选 → RICE 排序 → MoSCoW 收口）。优先级以"逐条双维标注"方式直接落表，不走三步串联。完整可填模板见 `1-Spec/prd.template.md` §7。

---

<a id="sec-6"></a>
## 六 版本化 / 迭代 / 变更管理

SDIE 的 Spec 材料通过三层机制实现可追溯、可迭代、可变更：

<a id="sec-6-1"></a>
### 6.1 元信息七字段（空间约定，所有 Spec 材料第 1 节挂此元数据）

| 字段 | 含义 | 示例 |
|------|------|------|
| `id` | 文档唯一标识 | DOC-SPEC-GUIDE-001 / TASK-CART-001 |
| `title` | 标题 | SDIE Spec 阶段工作指南 |
| `status` | 生命周期状态（见 [6.2](#sec-6-2) 状态机） | draft |
| `phase` | 所属 SDIE 阶段 | Spec |
| `owner` | 责任人类角色 | PM/PO |
| `related_docs` | 关联文档（双向链接） | [PRD-checkout, AC-cart] |
| `last_updated` | 最后更新日期 | 2026-08-05 |

<a id="sec-6-2"></a>
### 6.2 文档状态机（status 字段取值与流转）

```
draft ──(需求澄清会 + 评审通过)──▶ review
review ──(PM 签 Gate 1)──────────▶ baseline（基线化，进入 Design 的准入）
baseline ──(发现需修改)───────────▶ change（开变更，生成新版本号）
change ──(变更评审通过)───────────▶ baseline（新基线）
baseline ──(被新方案替代)─────────▶ superseded by <new-id>
```

> **基线化（baseline）是硬门槛**：只有 status=baseline 的 Spec 才能过 Gate 1 进入 Design。
> 一旦 baseline，内容不可原地涂改；任何修改都走 change 流程并产出新版本（[6.5](#sec-6-5)）。

<a id="sec-6-3"></a>
### 6.3 SemVer 版本号（权威推荐，semver.org）

对 Spec 文档/包采用 `MAJOR.MINOR.PATCH`：

| 递增 | 触发条件（Spec 语境） |
|------|----------------------|
| **MAJOR** | 不兼容的需求变更：删除/重构核心验收语义、业务目标反转、范围重大收缩 |
| **MINOR** | 向后兼容的新增：补充新用户故事、新增非破坏性 AC、扩展故事地图分支 |
| **PATCH** | 向后兼容的修正：错别字、表述澄清、AC 阈值微调（不改变行为预期） |

规则（引自 SemVer 2.0.0）：版本一经发布内容不可修改，修改必须发新版本；
`0.y.z` 为初始开发期（API/语义未稳定），`1.0.0` 起语义稳定。预发布可用 `-alpha` / `-rc` 后缀。

<a id="sec-6-4"></a>
### 6.4 ADR 决策记录（权威推荐，Michael Nygard 2011）

当 Spec 阶段出现**需要追溯的架构/范围/验收口径决策**（如"为何把超卖边界推迟到 Implement"），
写一条 ADR，格式：

```
# ADR-00XX：<决策标题>
## Status: Proposed | Accepted | Superseded by ADR-00YY
## Context: 什么力量/约束迫使此决策（业务目标、合规、技术债…）
## Decision: 我们决定…
## Consequences: 正向 / 负向 / 中性后果
## Alternatives: 被否决的备选及其理由
```

ADR 不可涂改，替代时新建并标 `Superseded by`。落位 `docs/adr/`。

<a id="sec-6-5"></a>
### 6.5 基线化变更流程（Spec 迭代闭环）

1. 任何对 baseline Spec 的修改请求 → 开 `change` 状态副本，记录变更动机（可挂 ADR）。
2. 按 [§3](#sec-3) 递进流程重新走：PM 改稿（R）→ SME/Tech Lead/QA 审（C）→ PM 定稿（A）。
3. 版本号按 [6.3](#sec-6-3) 递增；`last_updated` 刷新；`related_docs` 互链新旧版本。
4. 重新签 Gate 1（PM 审批，QA 复核验收可测）→ 新 baseline。
5. 旧版本置 `superseded by <new-id>`，保留考古记录，不删除。

> **版本历史段落（必填，方案 A）**：每一篇基线化的阶段产出物，须在**正文末尾**维护 `## 版本历史` 段落，
> 把"版本号"与"该版本的具体变化"钉合——满足 IEEE 828 变更记录要求与 Keep a Changelog 惯例
> （权威溯源见 0-References/changelog.md（Keep a Changelog 惯例））。
> 本段落**不进元信息七字段**（七字段保持不变），避免每篇元数据膨胀；版本号本身由 [6.3](#sec-6-3) 规范推导。
> 每条记录格式：`| 版本 | 日期 | 变更摘要 | 关联 ADR（可选）|`，与状态机 [6.2](#sec-6-2)、版本号 [6.3](#sec-6-3) 协同。
>
> 模板示例（放入文档正文末尾）：
> ```markdown
> ## 版本历史
> | 版本 | 日期 | 变更摘要 | 关联 ADR |
> |------|------|----------|----------|
> | 1.0.0 | 2026-08-08 | 初始 baseline：PRD/用户故事地图/AC/TASK 齐备，过 Gate 1 | — |
> | 1.1.0 | 2026-08-12 | MINOR：补充"游客结账"用户故事（向后兼容新增） | ADR-0012 |
> | 2.0.0 | 2026-08-20 | MAJOR：删除超卖边界语义，范围重大收缩 | ADR-0015 |
> ```

<a id="sec-6-6"></a>
### 6.6 Git 约定（权威推荐实践，落地建议）

| 约定 | 规则 |
|------|------|
| 分支 | `spec/<feature>` 开发分支；基线后合入 `main` 并经 Gate 1 门禁 |
| 提交类型 | `spec:`（新增/修订规格）、`adr:`（决策记录）、`docs:`（辅助文档） |
| 标签 | 基线版本打 `vX.Y.Z`（如 `v1.2.0`） |
| 不可委托 | 合并审批（⑦）由 PO 在 Gate 3 才发生；Spec 阶段仅做文档评审，不触发代码合并 |

---

<a id="sec-7"></a>
## 七 Gate 1 准入检查清单

> 本清单是 `SDIE-RACI-Matrix.md:345` Gate 1 准入标准（**Task Spec 完整、AGENTS.md 最新、验收可测**）的可执行化。
> PM 作为 A 必须**逐项确认通过**后方可签 Gate 1 放行进 Design；QA 作为 C 把关"验收可测"。
>
> 在执行完 [§3](#sec-3)（工作流程）与 [§4](#sec-4)（产出物字段级内容）后，对照本清单逐项核对。

| # | 检查项 | 对应产出物 / 依据 | 不可委托红线 |
|---|--------|------------------|--------------|
| 1 | **Task Spec 完整**：PRD / 用户故事地图 / `acceptance_criteria` / `TASK-*.yaml` 四类齐备，均挂元信息七字段（MD 第 1 节 / YAML `meta:` 块）；`TASK-*.yaml` 的 why/what/out 为人类手写 | `1-Spec/*` 模板；[§4](#sec-4) | ① 业务需求与验收语义拍板（PM/SME） |
| 2 | **AGENTS.md 最新**：执行边界承载与最新 RACI 同步，含本任务上下文与不可委托红线 | `AGENTS.md`；[§2.1](#sec-2-1) | ⑩ Harness 维护（Dev+TL） |
| 3 | **验收可测**：`acceptance_criteria` 具备 Given/When/Then 或正反例，QA 作为 C 确认可测 | `acceptance_criteria`；[§4.3](#sec-4-3) | ① 验收语义拍板（PM/SME） |
| 4 | **优先级已冻结**：PRD 功能清单逐条双维标注（KANO+MoSCoW+理由+版本），Gate 1 前冻结，无串联决策 | `prd.template.md` §7；[§5.2](#sec-5-2) | ① 定级签字（PM/A） |
| 5 | **Goal↔Impact 双向 trace**：§1 每条 `GOAL-x` 至少有 §4 一条 `IMP-x` 支撑（§4.1 矩阵对应行非全空）；每条 `IMP-x` 的"关联 Goal"列填的是 `GOAL-x` 编号而非自然语言或"同上" | `prd.template.md` §1/§4/§4.1 | ① 业务需求与验收语义拍板（PM/SME） |
| 6 | **基线化完成**：Spec 包 `status=baseline`（非 draft/review） | [§6.2](#sec-6-2) 状态机 | 只有 baseline 才能过 Gate 1 |

> **放行动作**：PM 在 #1–#5 全部 ✅ 后，于 PR 描述或 `1-Spec/README.md` 签 **Gate 1 Approved**，Spec 包方可进入 Design。任一 ❌ 即退回对应 R 修正，**不得带伤过门**。

---

<a id="sec-8"></a>
## 八 引用与权威来源

### 8.1 工作空间（SDIE 事实唯一内部权威）
- `SDIE-RACI-Matrix.md`
  - §1 RACI 基础约定（A 永属人类，Agent 仅 R）
  - §3.1 Spec 阶段 RACI
  - §3.5.1 Spec 阶段动作级工作清单
  - §4 AI Agent 执行矩阵（Spec Agent 落位 `docs/specs/task-specs/*.yaml`）
  - §5 不可委托清单 ①（业务需求与验收语义拍板 → PM/SME）
  - §6 Gate 1（Spec→Design，PM 审批，QA 为 C 把关验收可测）
- `SDIE-Analysis.md`：SDIE 四阶段、RACI 改造、Gate 1–4 导读

### 8.2 外部权威方法论（经 WebSearch 取得，标注出处）
| 方法论 | 权威来源 |
|--------|----------|
| User Story Mapping | Jeff Patton, *User Story Mapping* (O'Reilly 2014)；jpattonassociates.com |
| INVEST | Bill Wake (2003), "INVEST in Good Stories", xp123.com/articles/invest-in-good-stories |
| BDD / Given-When-Then / Gherkin | Dan North (2006) "Introducing BDD"；Cucumber 官方 cucumber.io/docs/gherkin |
| Example Mapping | Matt Wynne (Cucumber), cucumber.io/blog/bdd/example-mapping-introduction |
| Job Stories | Paul Adams & Alan Klement @ Intercom (2013), blog.intercom.io/accidentally-invented-job-stories |
| Impact Mapping | Gojko Adzic (2012), impactmapping.org |
| Event Storming | Alberto Brandolini (2013), eventstorming.com |
| MoSCoW | Dai Clegg (1994) / DSDM, Agile Business Consortium agilebusiness.org |
| KANO Model | Noriaki Kano (1984), "Attractive Quality and Must-Be Quality" |
| Semantic Versioning 2.0.0 | semver.org |
| Architecture Decision Record (ADR) | Michael Nygard (2011), "Documenting Architecture Decisions", cognitect.com/blog/2011/11/15/documenting-architecture-decisions |

---

<a id="sec-9"></a>
## 九 五问一句话总结

- **① 工作**：PM 起草需求与验收标准（R）→ SME/Tech Lead/QA 评审语义与可行性（C）→ PM 定稿并签 Gate 1（A）→ Dev 起草 TASK-*.yaml（R）；安全/Reviewer 知会（I）。
- **② 材料**：PRD、用户故事地图、`acceptance_criteria`、`TASK-*.yaml` 四类，均挂元信息七字段（MD 第 1 节 / YAML `meta:` 块）。
- **③ 方法论**：空间已定义（元信息七字段、why/what/out 人类手写、Gate 1 验收可测把关）；权威推荐补足（Impact Mapping / User Story Mapping / INVEST / BDD / Example Mapping / Job Stories / Event Storming / MoSCoW / KANO）。
- **④ 内容**：见 [§4](#sec-4) 字段级示例——PRD 七段、故事地图四层、`acceptance_criteria` 正反例 + Gherkin、TASK-*.yaml 的 why/what/out 强制人类手写。
- **⑤ 版本化**：元信息七字段 + status 状态机（draft→review→baseline→change→superseded）+ SemVer 版本号 + ADR 决策记录 + Git 基线化变更闭环 + 正文 `## 版本历史` 段落（§6.5，方案 A）。

---

> 本文由产品战略团队 AI 协作生成，SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；
> 重要决策（尤其 Gate 1 审批与不可委托项 ①）请由产品负责人审定。
