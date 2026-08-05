---
id: DOC-SPEC-GUIDE-001
title: SDIE Spec 阶段工作指南（工作项 / 产出物 / 方法论 / 字段级内容 / 版本化管理）
status: draft
phase: Spec
owner: PM/PO（产品负责人）
related_docs:
  - SDIE-RACI-Matrix.md
  - SDIE-Analysis.md
last_updated: 2026-08-05
---

# SDIE Spec 阶段工作指南

> 本文是 SDIE 工程方法在 **Spec（规格）阶段** 的操作手册，系统回答五个问题：
> ① Spec 应执行哪些工作？② 分别应输出哪些材料？③ 可用什么方法论产出这些材料？
> ④ 材料内容具体有哪些（字段级）？⑤ 如何版本化、迭代与管理？
>
> **权威来源约定（用户硬约束）**：本文所有 SDIE 事实仅取自当前工作空间
> `SDIE-RACI-Matrix.md`（7 人类 / 12 Agent 治理版，唯一权威明细）与 `SDIE-Analysis.md`，
> 任何本地其他目录（如 SDIE/、SDIE1/）均不引用。方法论与外部实践通过 WebSearch 取得权威定义并标注出处。

---

## 0. 一图速览：五问与本文对应

| 你的问题 | 本文回答章节 |
|----------|--------------|
| ① Spec 应执行哪些工作？ | §2 Spec 阶段定位与门禁、§3 递进工作清单（R/A/C/I） |
| ② 分别应输出哪些材料？ | §4 输出材料清单 |
| ③ 可用什么方法论产出这些材料？ | §5 方法论映射（空间已定义 vs 权威推荐） |
| ④ 材料内容具体有哪些？ | §6 各材料字段级示例 |
| ⑤ 如何版本化、迭代与管理？ | §7 版本化 / 迭代 / 变更管理 |

---

## 1. SDIE 与 Spec 阶段定位

SDIE = **S**pec / **D**esign / **I**mplement / **E**valuation，是面向 AI Coding 的治理框架。
其关键改造（见 `SDIE-RACI-Matrix.md` §1）：**A（问责 / 批准权）永远由人类承担；AI Agent 仅作为 R（执行者）**——
因为当代码可被秒级重写时，真正有价值的是"什么是对的"这一判断，必须由人做出。

四阶段中，Spec 是**源头与意图锚点**：它把模糊的业务诉求收敛为结构化、可验证、可追责的需求与验收语义，
并通过 **Gate 1** 进入 Design 阶段。

### Gate 1：Spec → Design 的准入门禁（§6）

| 项目 | 内容 |
|------|------|
| 准入标准 | Task Spec 完整、AGENTS.md 最新、验收可测 |
| 人类审批人（A） | **PM**（产品经理 / 产品负责人） |
| 咨询方（C） | **QA**——把关"验收可测"（acceptance-testability） |
| 不可委托项 | ① 业务需求与验收语义拍板（PM/SME） |

> 关键纪律：Gate 1 的 A 是 PM，不是 Tech Lead。架构选型（②）留到 Design 阶段，Spec 阶段
> Tech Lead 仅做**架构可行性初判**（C），不担 A（§3.1 / §3.5.1）。

---

## 2. Spec 阶段 RACI 速查（引自 §3.1）

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
> 落位 `docs/specs/task-specs/*.yaml`；但所有产出需人类（PM/Dev）复核（§4）。

---

## 3. Spec 应执行的递进工作（动作级清单，引自 §3.5.1）

行序＝本阶段**递进执行顺序**：作者起草 → 协作咨询 → 问责定稿 → 知会。
`acceptance_criteria` 先由 PM/PO **撰写（R）**，再交 SME / Tech Lead / QA 评审（C），
最终由 PM/PO **定稿并签 Gate 1（A）**。

| # | 角色 | RACI | 具体工作项 |
|---|------|:----:|-----------|
| 1 | PM/PO | **R** | 将 SME / 业务方口述整理为结构化需求初稿；逐条撰写 `acceptance_criteria`（用可验证语言定义 done）；维护 PRD 文档与版本 |
| 2 | SME | **C** | 逐条审 `acceptance_criteria` 领域语义；指出 Agent 易编造的伪边界条件；确认术语定义无歧义 |
| 3 | Tech Lead | **C** | 对需求做技术可行性初判；标注高风险技术点供 Design 阶段深入 |
| 4 | QA | **C** | 评估测试可行性与风险；初步建议测试策略与门禁阈值草案 |
| 5 | PM/PO | **A** | 主持需求澄清会，对齐业务目标与边界；标注 `out_of_scope` 防范围蔓延；**签 Gate 1**（需求评审通过）—— 对定稿的 `acceptance_criteria` 负最终责 |
| 6 | Dev (Task Owner) | **R** | 基于 PRD 起草 `TASK-*.yaml` 骨架（why/what/out 必须人类写）；标注 `agent_hint` 与上下文来源；提交 Spec Agent 草稿供 PM 审核 |
| 7 | 安全 / 红队 | **I** | 知会范围；预判安全关注点（认证 / 数据 / 合规） |
| 8 | Reviewer | **I** | 知会需求；记录潜在审查关注点，便于 Implement 阶段对齐 |

**核心不可委托红线（§5 ①）**：业务需求与验收语义拍板必须由 PM / SME 亲自完成，
Agent 不得定义领域规则或拍板需求（§2.1 角色说明）。

---

## 4. 输出材料清单

Spec 阶段的产出物（引自 §3.1 / §3.5.1 / §4）共四类，均需在 frontmatter 挂七字段
（id / title / status / phase / owner / related_docs / last_updated，见 §7）：

| 材料 | 主要作者（R） | 问责者（A） | 关键读者（C/I） | 落位（参考） |
|------|--------------|------------|----------------|-------------|
| **PRD**（产品需求文档） | PM/PO（R） | PM/PO（A，①） | SME(C)、Tech Lead(C)、QA(C) | `docs/specs/` |
| **用户故事地图 User Story Map** | PM/PO + Dev（R） | PM/PO（A） | SME(C)、Tech Lead(C) | `docs/specs/story-map/` |
| **`acceptance_criteria`**（验收标准集） | PM/PO（R，逐条撰写） | PM/PO（A，①） | SME(C 语义)、QA(C 可测) | 内嵌于 PRD / TASK-SPEC |
| **`TASK-*.yaml`**（结构化任务规格） | Dev (Task Owner)（R） | PM/PO（A，Gate 1） | Tech Lead(C)、QA(C)、Reviewer(I) | `docs/specs/task-specs/*.yaml` |

> 注：四类材料不是平铺的"四份独立文件"，而是**同一意图的不同抽象层**——PRD 是业务层、
> 故事地图是用户旅程层、`acceptance_criteria` 是验证层、`TASK-*.yaml` 是执行层。
> 它们通过 frontmatter 的 `related_docs` 互相链接，形成可追溯的 Spec 包。

---

## 5. 可用方法论映射（空间已定义 vs 权威推荐）

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

**原则**：空间已定义的（frontmatter 七字段、why/what/out 人类手写、Gate 1 验收可测把关、不可委托 ①）必须执行；
缺口的方法论以权威推荐形式补足，团队可裁剪但需记录在 ADR 中。

---

## 6. 各材料具体内容（字段级示例）

### 6.1 PRD（产品需求文档）

`frontmatter` 七字段见 §7。正文建议结构：

```
# PRD：<功能名>
## 1. 背景与目标（Business Goal）
   - 目标（用 Impact Mapping 的 Goal 写法，SMART）：
     "在 Q3 将购物车放弃率从 68% 降到 55%"
## 2. 范围（Scope）
   - in_scope: [...]
   - out_of_scope: [...]   ← PM 在 Gate 1 标注，防范围蔓延（§3.5.1 #5）
## 3. 用户与角色（Actors）
   - 主要角色：...；次要角色：...； off-stage：合规/支付网关
## 4. 用户故事地图摘要（链接 story-map 文档）
## 5. 验收标准集（嵌入 acceptance_criteria，见 6.3）
## 6. 非目标与假设（Non-goals / Assumptions）
## 7. 优先级（MoSCoW + KANO 标注）
```

### 6.2 用户故事地图（User Story Mapping 字段）

| 层级 | 字段 | 说明 |
|------|------|------|
| Backbone | `activity` | 主干活动（左→右时序），如"浏览→加购→结算→管理订单" |
| | `step` | 活动下的用户任务，如"加购"下的"查看加购按钮/点击加购" |
| Details | `story` | 实现细节，高优先置顶、低优先置底 |
| Release slices | `release` | 横向分割线，MVP 在顶部，未来增强在底部 |
| Out-of-scope | `deferred` | 明确"暂不做"的项，单独区放置 |

### 6.3 `acceptance_criteria`（验收标准——正反例）

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

### 6.4 `TASK-*.yaml`（结构化任务规格）

> **红线**：`why` / `what` / `out` 三个字段**必须人类手写**，Agent 仅可补 `agent_hint` 等草稿
> （§3.5.1 #6 / §4 Spec Agent 落位）。

```yaml
id: TASK-CART-001
title: 购物车加入商品接口
# ---- frontmatter 七字段 ----
status: draft            # draft → review → baseline → change → superseded
phase: Spec
owner: dev-zhang         # Task Owner
related_docs:
  - PRD-checkout-2026-08.md
  - AC-cart.md
last_updated: 2026-08-05
# ---- 人类强制手写核心三字段 ----
why: 购物车放弃率高，需在详情页提供一键加购以降低流失   # 业务动机，人类写
what: 提供 POST /cart/add 接口，入参 sku+qty，出参购物车快照 # 做什么，人类写
out: 加购成功返回 200 + 购物车快照；库存不足返回 409      # 期望产出/验收，人类写
# ---- Agent 可补草稿（需人类审核）----
agent_hint: 参考现有 order 服务的事务写法；注意并发库存校验
context_sources:
  - src/checkout/CartService.java
  - docs/design/cart-ADR-003.md
```

---

## 7. 版本化 / 迭代 / 变更管理

SDIE 的 Spec 材料通过三层机制实现可追溯、可迭代、可变更：

### 7.1 frontmatter 七字段（空间约定，所有 Spec 材料挂此元数据）

| 字段 | 含义 | 示例 |
|------|------|------|
| `id` | 文档唯一标识 | DOC-SPEC-GUIDE-001 / TASK-CART-001 |
| `title` | 标题 | SDIE Spec 阶段工作指南 |
| `status` | 生命周期状态（见 7.2 状态机） | draft |
| `phase` | 所属 SDIE 阶段 | Spec |
| `owner` | 责任人类角色 | PM/PO |
| `related_docs` | 关联文档（双向链接） | [PRD-checkout, AC-cart] |
| `last_updated` | 最后更新日期 | 2026-08-05 |

### 7.2 文档状态机（status 字段取值与流转）

```
draft ──(需求澄清会 + 评审通过)──▶ review
review ──(PM 签 Gate 1)──────────▶ baseline（基线化，进入 Design 的准入）
baseline ──(发现需修改)───────────▶ change（开变更，生成新版本号）
change ──(变更评审通过)───────────▶ baseline（新基线）
baseline ──(被新方案替代)─────────▶ superseded by <new-id>
```

> **基线化（baseline）是硬门槛**：只有 status=baseline 的 Spec 才能过 Gate 1 进入 Design。
> 一旦 baseline，内容不可原地涂改；任何修改都走 change 流程并产出新版本（7.3）。

### 7.3 版本号：SemVer 2.0.0（权威推荐，semver.org）

对 Spec 文档/包采用 `MAJOR.MINOR.PATCH`：

| 递增 | 触发条件（Spec 语境） |
|------|----------------------|
| **MAJOR** | 不兼容的需求变更：删除/重构核心验收语义、业务目标反转、范围重大收缩 |
| **MINOR** | 向后兼容的新增：补充新用户故事、新增非破坏性 AC、扩展故事地图分支 |
| **PATCH** | 向后兼容的修正：错别字、表述澄清、AC 阈值微调（不改变行为预期） |

规则（引自 SemVer 2.0.0）：版本一经发布内容不可修改，修改必须发新版本；
`0.y.z` 为初始开发期（API/语义未稳定），`1.0.0` 起语义稳定。预发布可用 `-alpha` / `-rc` 后缀。

### 7.4 决策记录：ADR（权威推荐，Michael Nygard 2011）

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

### 7.5 基线化变更流程（Spec 迭代闭环）

1. 任何对 baseline Spec 的修改请求 → 开 `change` 状态副本，记录变更动机（可挂 ADR）。
2. 按 §3 递进流程重新走：PM 改稿（R）→ SME/Tech Lead/QA 审（C）→ PM 定稿（A）。
3. 版本号按 7.3 递增；`last_updated` 刷新；`related_docs` 互链新旧版本。
4. 重新签 Gate 1（PM 审批，QA 复核验收可测）→ 新 baseline。
5. 旧版本置 `superseded by <new-id>`，保留考古记录，不删除。

### 7.6 Git 约定（权威推荐实践，落地建议）

| 约定 | 规则 |
|------|------|
| 分支 | `spec/<feature>` 开发分支；基线后合入 `main` 并经 Gate 1 门禁 |
| 提交类型 | `spec:`（新增/修订规格）、`adr:`（决策记录）、`docs:`（辅助文档） |
| 标签 | 基线版本打 `vX.Y.Z`（如 `v1.2.0`） |
| 不可委托 | 合并审批（⑦）由 PO 在 Gate 3 才发生；Spec 阶段仅做文档评审，不触发代码合并 |

---

## 8. 引用与权威来源清单

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

## 9. 五问一句话总结

- **① 工作**：PM 起草需求与验收标准（R）→ SME/Tech Lead/QA 评审语义与可行性（C）→ PM 定稿并签 Gate 1（A）→ Dev 起草 TASK-*.yaml（R）；安全/Reviewer 知会（I）。
- **② 材料**：PRD、用户故事地图、`acceptance_criteria`、`TASK-*.yaml` 四类，均挂 frontmatter 七字段。
- **③ 方法论**：空间已定义（七字段、why/what/out 人类手写、Gate 1 验收可测把关）；权威推荐补足（Impact Mapping / User Story Mapping / INVEST / BDD / Example Mapping / Job Stories / Event Storming / MoSCoW / KANO）。
- **④ 内容**：见 §6 字段级示例——PRD 七段、故事地图四层、`acceptance_criteria` 正反例 + Gherkin、TASK-*.yaml 的 why/what/out 强制人类手写。
- **⑤ 版本化**：frontmatter 七字段 + status 状态机（draft→review→baseline→change→superseded）+ SemVer 版本号 + ADR 决策记录 + Git 基线化变更闭环。

---

> 本文由产品战略团队 AI 协作生成，SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；
> 重要决策（尤其 Gate 1 审批与不可委托项 ①）请由产品负责人审定。
