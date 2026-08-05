---
id: DOC-DESIGN-GUIDE-001
title: SDIE Design 阶段工作指南（工作项 / 产出物 / 方法论 / 字段级内容 / 版本化管理）
status: draft
phase: Design
owner: Tech Lead / 架构负责人
related_docs:
  - SDIE-RACI-Matrix.md
  - SDIE-Analysis.md
  - SDIE-Spec-Guide.md
last_updated: 2026-08-06
---

# SDIE Design 阶段工作指南

> 本文是 SDIE 工程方法在 **Design（设计）阶段** 的操作手册，系统回答五个问题：
> ① Design 应执行哪些工作？② 分别应输出哪些材料？③ 可用什么方法论产出这些材料？
> ④ 材料内容具体有哪些（字段级）？⑤ 如何版本化、迭代与管理？
>
> **权威来源约定（用户硬约束）**：本文所有 SDIE 事实仅取自当前工作空间
> `SDIE-RACI-Matrix.md`（7 人类 / 12 Agent 治理版，唯一权威明细）与 `SDIE-Analysis.md`。
> 方法论与外部实践通过权威渠道取得并标注出处。

---

## 0. 一图速览：五问与本文对应

| 你的问题 | 本文回答章节 |
|----------|--------------|
| ① Design 应执行哪些工作？ | §2 Design 阶段定位与门禁、§3 递进工作清单（R/A/C/I） |
| ② 分别应输出哪些材料？ | §4 输出材料清单 |
| ③ 可用什么方法论产出这些材料？ | §5 方法论映射（空间已定义 vs 权威推荐） |
| ④ 材料内容具体有哪些？ | §6 各材料字段级示例 |
| ⑤ 如何版本化、迭代与管理？ | §7 版本化 / 迭代 / 变更管理 |

---

## 1. SDIE 与 Design 阶段定位

SDIE = **S**pec / **D**esign / **I**mplement / **E**valuation，是面向 AI Coding 的治理框架。
其关键改造（见 `SDIE-RACI-Matrix.md` §1）：**A（问责 / 批准权）永远由人类承担；AI Agent 仅作为 R（执行者）**。

四阶段中，Design 是**技术锚点**：它把 Spec 收敛出的结构化需求与 `TASK-*.yaml`，转化为
**架构选型、ADR 定稿、上下文注入策略、以及 Agent 可独立完成的原子任务分解**，并通过 **Gate 2** 进入 Implement 阶段。

### Gate 2：Design → Implement 的准入门禁（§6）

| 项目 | 内容 |
|------|------|
| 准入标准 | 分解合理、上下文策略就绪、Harness 确认 |
| 人类审批人（A） | **Tech Lead / 架构负责人** |
| 不可委托项 | ② 架构选型与 ADR 定稿（Tech Lead）；③ 门禁阈值设定（Tech Lead + QA） |

> 关键纪律：Gate 2 的 A 是 Tech Lead，不是 PM。架构选型（②）只在 Design 阶段定稿，
> Spec 阶段 Tech Lead 仅做**架构可行性初判**（C）；门禁阈值（覆盖率/变异/安全级别）由 Tech Lead 与 QA **共定**（③），
> 不能由任一方单独拍板（§3.2 / §3.5.2 / §2.1）。

---

## 2. Design 阶段 RACI 速查（引自 §3.2）

| 角色 | RACI | 本阶段定位 |
|------|:----:|-----------|
| 产品经理 PM/PO | **I** | 知会设计结论，确认不偏离业务目标 |
| 业务专家 SME | **I** | 知会设计对验收语义的落地方式 |
| Tech Lead / 架构负责人 | **R/A** | 主导架构审查、定稿 ADR、确认上下文注入策略；**Gate 2 审批担责**；架构选型与 ADR 定稿不可委托（②） |
| 开发工程师 Task Owner | **R** | 将 Task Spec 分解为 Agent 可独立完成的原子任务；设计上下文注入策略；产出分解方案 |
| 测试架构师 / QA | **C** | 制定测试策略、分级占比、门禁阈值草案（阈值最终设定不可委托 ③，与 Tech Lead 共定） |
| 安全 / 红队 | **C** | 咨询安全设计点（认证 / 授权 / 数据保护），预埋扫描关注项 |
| 评审官 Reviewer | **I** | 知会设计决策，便于后续 Review 对齐分层与契约 |

> Agent 侧：Design Agent 在 Design 阶段为 **● 主导执行**（R 侧），产出原子分解草案、ADR 草稿，
> 落位 `docs/design/`；但所有产出需人类（Tech Lead）复核（§4）。Tech Lead 可让 Design Agent 起草，
> **但架构选型与 ADR 定稿不可委托（②）**，且不得让 Agent 改安全配置（§2.1）。

---

## 3. Design 应执行的递进工作（动作级清单，引自 §3.5.2）

行序＝本阶段**递进执行顺序**：作者主导 → 协作分解/咨询 → 问责定稿 → 知会。

| # | 角色 | RACI | 具体工作项 |
|---|------|:----:|-----------|
| 1 | Tech Lead / 架构负责人 | **R/A** | 主导架构审查，产出定稿 ADR；定义上下文注入策略（Agent 需哪些 repo / 文档）；签 Gate 2（架构评审通过）；评估技术债与取舍（不可委托 ②） |
| 2 | 开发工程师 (Task Owner) | **R** | 将 Task Spec 拆为 Agent 可独立完成的原子任务；设计任务间依赖与验收映射；产出分解方案文档 |
| 3 | 测试架构师 / QA | **C** | 制定测试策略（分级 / 占比）；拟定门禁阈值草案（覆盖率 / 变异）（③ 与 Tech Lead 共定）；设计测试数据方案 |
| 4 | 安全 / 红队 | **C** | 咨询安全设计点（认证 / 授权 / 数据保护）；标注需重点红队演练的攻击面 |
| 5 | 产品经理 (PM/PO) | **I** | 知会设计决策；确认不偏离业务目标 |
| 6 | 业务专家 (SME) | **I** | 知会验收语义落地方式；反馈领域约束是否被满足 |
| 7 | 评审官 (Reviewer) | **I** | 知会设计决策；预判审查风险点 |

**核心不可委托红线**：
- **② 架构选型与 ADR 定稿**必须由 Tech Lead 亲自完成，Agent 只能起草（§2.1 / §5 ②）。
- **③ 门禁阈值设定**（覆盖率/变异/安全级别）必须由 Tech Lead + QA 共定，不能单方拍板。
- **⑩ Harness 维护**（若本次设计涉及 AGENTS.md / 校验脚本改动）由 Dev + Tech Lead 负责，非 Agent 自改。

---

## 4. 输出材料清单

Design 阶段的产出物（引自 §3.2 / §3.5.2 / §4）共五类，均需在 frontmatter 挂七字段
（id / title / status / phase / owner / related_docs / last_updated，见 §7）：

| 材料 | 主要作者（R） | 问责者（A） | 关键读者（C/I） | 落位（参考） |
|------|--------------|------------|----------------|-------------|
| **ADR**（架构决策记录） | Tech Lead（R，Design Agent 起草草稿） | Tech Lead（A，②） | Dev(C)、QA(C)、安全(C)、Reviewer(I) | `docs/design/` |
| **原子分解方案**（Decomposition） | Dev (Task Owner)（R） | Tech Lead（A，Gate 2） | Design Agent(执行)、QA(C) | `docs/design/` 或随 TASK |
| **测试策略 + 门禁阈值草案** | QA（C，草案） | Tech Lead + QA（A，③ 共定） | Dev(C)、安全(C) | `docs/design/test-strategy.md` |
| **上下文注入策略** | Tech Lead（R/A） | Tech Lead（A） | Dev(R)、Design Agent(消费) | `docs/design/context-injection.md` |
| **安全设计点** | 安全 / 红队（C） | 安全 / 红队（判定权 ⑤） | Tech Lead(C)、Dev(I) | `docs/design/security-design.md` |

> 注：五类材料是**同一架构决策的不同切面**——ADR 是"为何这样选"，原子分解是"怎么拆给 Agent 做"，
> 测试策略是"怎么验"，上下文注入是"Agent 能看什么"，安全设计点是"红线在哪"。
> 它们通过 frontmatter 的 `related_docs` 互相链接，形成可追溯的 Design 包。

---

## 5. 可用方法论映射（空间已定义 vs 权威推荐）

> 标记说明：**【空间已定义】**＝SDIE 工作空间内已规定；**【权威推荐】**＝经权威渠道取得的外部实践，作为补充方法论。

| 产出物 / 工作 | 推荐方法论 | 来源性质 | 权威出处 |
|---------------|-----------|----------|----------|
| ADR 格式与不可委托 ② | **ADR**（Architecture Decision Record） | 【空间已定义】② + 【权威推荐】 | `SDIE-RACI-Matrix.md` §3.2/§5②；Michael Nygard 2011，cognitect.com/blog/2011/11/15/documenting-architecture-decisions |
| 架构可视化 / 上下文-容器-组件 | **C4 Model**（Context/Container/Component/Code） | 【权威推荐】 | Simon Brown，c4model.com |
| 领域边界划分 | **DDD 限界上下文**（Bounded Context） | 【权威推荐】 | Eric Evans，*Domain-Driven Design* (2003)；martinfowler.com/bliki/BoundedContext |
| 多视图架构表达 | **4+1 视图模型**（Logical/Process/Development/Physical + Scenarios） | 【权威推荐】 | Philippe Kruchten 1995，IEEE Software |
| 测试策略 / 门禁阈值草案（③） | **Test Pyramid / Test Strategy**（单元/集成/端到端 分级占比） | 【权威推荐】 | Mike Cohn；martinfowler.com/articles/practical-test-pyramid |
| 安全设计点（⑤ 判定权在手） | **STRIDE 威胁建模**（Spoofing/Tampering/Repudiation/InfoDisclosure/DenialOfService/Elevation） | 【权威推荐】 | Microsoft，learn.microsoft.com/.../security/thread-modeling |
| 上下文注入策略设计 | 【空间已定义】Tech Lead 定义 Agent 可读文件集合 | 【空间已定义】 | `SDIE-RACI-Matrix.md` §3.2 / §2.1 |
| 版本化 / 迭代 / 变更 | **SemVer 2.0.0** + **ADR** | 【权威推荐】 | semver.org；Michael Nygard 2011 |

**原则**：空间已定义的（frontmatter 七字段、② 架构选型与 ADR 定稿 human-only、③ 阈值共定、上下文注入策略）必须执行；
缺口的方法论以权威推荐形式补足，团队可裁剪但需记录在 ADR 中。

---

## 6. 各材料具体内容（字段级示例）

### 6.1 ADR（架构决策记录）

`frontmatter` 七字段见 §7；正文采用 Nygard 四段式：

```
# ADR-DESIGN-012：购物车服务采用领域事件驱动
## Status: Accepted
## Context: 加购/结算/库存需解耦；现有同步调用导致超时与库存超卖风险
## Decision: 引入领域事件（CartAdded / OrderCreated），购物车写、库存与结算订阅消费
## Consequences:
  + 解耦、可独立扩容；- 需引入消息可靠投递与幂等设计
## Alternatives: 同步 RPC（否决：耦合高、超时传播）
```

> 红线：ADR 的 **Status/Decision 由 Tech Lead 定稿（②）**，Design Agent 仅起草；ADR 不可原地涂改，替代时标 `Superseded by`。

### 6.2 原子分解方案（Decomposition）

| 字段 | 说明 | 示例 |
|------|------|------|
| `task_id` | 原子任务标识（挂 TASK-*.yaml 关联） | DECOMP-CART-001 |
| `title` | 任务名 | 购物车加入商品接口 |
| `agent_assignable` | 是否可由 Coding Agent 独立完成 | true / false（false=需人类写骨架） |
| `depends_on` | 依赖的前置任务 | [DECOMP-CART-000] |
| `acceptance_ref` | 对应 `acceptance_criteria` 编号 | AC-1, AC-2 |
| `context_scope` | 该任务允许 Agent 读取的文件/目录（上下文注入） | src/checkout/*, docs/design/cart-ADR-012.md |

### 6.3 测试策略 + 门禁阈值草案（Test Strategy）

```
## 测试分级占比（草案，③ 与 Tech Lead 共定）
- 单元测试(unit): 70%  | 集成(integration): 20% | 端到端(e2e): 10%
## 门禁阈值草案（待共定后写入 Harness）
- 覆盖率(coverage): ≥ 80%
- 变异分(mutation/PITest): ≥ 60%   ← ④·验证 的核心证据，非仅覆盖率
- 安全: 0 高危 (0 high)
## 测试数据方案: 用影子库 + 工厂方法，禁止生产数据入测试
```

### 6.4 上下文注入策略（Context Injection）

| 字段 | 说明 | 示例 |
|------|------|------|
| `allow_read` | Agent 可读取的路径/文档 | src/checkout/*, docs/design/*, ADR-012 |
| `deny_read` | 禁止 Agent 读取（防越权/泄漏） | secrets/, .env, 其他租户模块 |
| `inject_via` | 注入方式 | PR 描述 / 系统提示 / 文件引用 |
| `verify` | 注入后人类复核点 | Task Owner 审 Agent 输出是否越界读 |

### 6.5 安全设计点（Security Design）

```
## 认证 / 授权
- 加购接口需登录态 + 资源归属校验（越权 ⑤ 红线）
## 数据保护
- 购物车含 PII 时加密存储；日志脱敏
## 红队重点攻击面（标注供 Evaluation 演练）
- 提示注入篡改加购数量；越权读取他人购物车；并发超卖
```

---

## 7. 版本化 / 迭代 / 变更管理

Design 材料沿用 Spec 指南 §7 的三层机制（frontmatter 七字段 + 状态机 + SemVer + ADR + Git 闭环），并补充阶段专属门槛。

### 7.1 frontmatter 七字段（所有 Design 材料挂此元数据）
同 Spec 指南 §7.1：`id / title / status / phase / owner / related_docs / last_updated`。

### 7.2 文档状态机（status 字段取值与流转）
同 Spec 指南 §7.2：`draft → review → baseline → change → superseded`。

> **基线化（baseline）是硬门槛**：只有 status=baseline 的 Design 包（ADR 定稿 + 分解合理 + 上下文策略就绪 + Harness 确认）才能过 **Gate 2** 进入 Implement。
> 一旦 baseline，内容不可原地涂改；任何修改都走 change 流程并产出新版本。

### 7.3 版本号：SemVer 2.0.0（权威推荐，semver.org）
对 Design 文档/包采用 `MAJOR.MINOR.PATCH`：

| 递增 | 触发条件（Design 语境） |
|------|----------------------|
| **MAJOR** | 不兼容的架构反转：ADR 推翻重选、契约级破坏性变更 |
| **MINOR** | 向后兼容的新增：补充原子任务、新增非破坏性上下文范围 |
| **PATCH** | 向后兼容的修正：分解描述澄清、阈值微调（不改行为预期） |

### 7.4 决策记录：ADR（权威推荐，Michael Nygard 2011）
Design 阶段**每条架构选型本身就是一条 ADR**（不可委托 ②）。ADR 不可涂改，替代时新建并标 `Superseded by`。落位 `docs/design/`（与 §4 一致）。

### 7.5 基线化变更流程（Design 迭代闭环）
1. 任何对 baseline Design 包的修改请求 → 开 `change` 状态副本，记录动机（挂新 ADR 或 ADR 修订）。
2. 按 §3 递进流程重新走：Tech Lead 改稿（R）→ Dev/QA/安全 审（C）→ Tech Lead 定稿（A）。
3. 版本号按 7.3 递增；`last_updated` 刷新；`related_docs` 互链新旧版本。
4. 重新签 Gate 2（Tech Lead 审批，QA 复核阈值）→ 新 baseline。
5. 旧版本置 `superseded by <new-id>`，保留考古记录，不删除。

### 7.6 Git 约定（权威推荐实践，落地建议）
| 约定 | 规则 |
|------|------|
| 分支 | `design/<feature>` 开发分支；基线后合入 `main` 并经 Gate 2 门禁 |
| 提交类型 | `design:`（架构/分解）、`adr:`（决策记录）、`docs:`（辅助文档） |
| 标签 | 基线版本打 `vX.Y.Z` |
| 不可委托 | 架构选型与 ADR 定稿（②）由 Tech Lead 在 Gate 2 完成；阈值共定（③）；Harness 维护（⑩）由 Dev+TL |

---

## 8. 引用与权威来源清单

### 8.1 工作空间（SDIE 事实唯一内部权威）
- `SDIE-RACI-Matrix.md`
  - §1 RACI 基础约定（A 永属人类，Agent 仅 R）
  - §2 概览矩阵（Design 行：TL=A/R，QA=C，安全=C）
  - §2.1 Tech Lead / QA / 安全 角色说明（② 不可委托、③ 阈值共定、⑤ 判定权在手）
  - §3.2 Design 阶段 RACI
  - §3.5.2 Design 阶段动作级工作清单
  - §4 AI Agent 执行矩阵（Design Agent 落位 `docs/design/`）
  - §5 不可委托清单 ②③⑩
  - §6 Gate 2（Design→Implement，Tech Lead 审批）
- `SDIE-Analysis.md`：§3.2 Design 阶段详解、§6 不可委托、§7 Gate 2

### 8.2 外部权威方法论（经权威渠道取得，标注出处）
| 方法论 | 权威来源 |
|--------|----------|
| Architecture Decision Record (ADR) | Michael Nygard (2011), "Documenting Architecture Decisions", cognitect.com/blog/2011/11/15/documenting-architecture-decisions |
| C4 Model | Simon Brown, c4model.com |
| DDD Bounded Context | Eric Evans (2003), *Domain-Driven Design*; martinfowler.com/bliki/BoundedContext |
| 4+1 View Model | Philippe Kruchten (1995), IEEE Software "The 4+1 View Model" |
| Test Pyramid / Test Strategy | Mike Cohn; martinfowler.com/articles/practical-test-pyramid |
| STRIDE Threat Modeling | Microsoft, learn.microsoft.com (Security/Threat Modeling) |
| Semantic Versioning 2.0.0 | semver.org |

---

## 9. 五问一句话总结

- **① 工作**：Tech Lead 主导架构审查、定稿 ADR、定义上下文注入策略并签 Gate 2（R/A，②）→ Dev 拆原子任务（R）→ QA 拟测试策略与阈值草案（C，③ 共定）→ 安全咨询设计点（C，⑤ 判定权在手）→ PM/SME/Reviewer 知会（I）。
- **② 材料**：ADR、原子分解方案、测试策略+门禁阈值草案、上下文注入策略、安全设计点五类，均挂 frontmatter 七字段。
- **③ 方法论**：空间已定义（② ADR 定稿 human-only、③ 阈值共定、上下文注入策略）；权威推荐补足（C4 / DDD 限界上下文 / 4+1 视图 / Test Pyramid / STRIDE / ADR / SemVer）。
- **④ 内容**：见 §6 字段级示例——ADR 四段式、分解方案的 agent_assignable/depends_on/context_scope、测试策略分级占比+变异分、上下文注入 allow/deny、安全设计点清单。
- **⑤ 版本化**：frontmatter 七字段 + status 状态机（baseline 才能过 Gate 2）+ SemVer + ADR 决策记录 + Git 基线化变更闭环。

---

> 本文由 AI 协作生成，SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；
> 重要决策（尤其 Gate 2 审批与不可委托项 ②）请由 Tech Lead / 架构负责人审定。
