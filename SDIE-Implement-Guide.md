---
id: DOC-IMPLEMENT-GUIDE-001
title: SDIE Implement 阶段工作指南（工作项 / 产出物 / 方法论 / 字段级内容 / 版本化管理）
status: draft
phase: Implement
owner: 开发工程师 Task Owner / Tech Lead / QA / Reviewer / PO（三 A 共担）
related_docs:
  - SDIE-RACI-Matrix.md
  - SDIE-Analysis.md
  - SDIE-Spec-Guide.md
  - SDIE-Design-Guide.md
last_updated: 2026-08-06
---

# SDIE Implement 阶段工作指南

> 本文是 SDIE 工程方法在 **Implement（实现）阶段** 的操作手册，系统回答五个问题：
> ① Implement 应执行哪些工作？② 分别应输出哪些材料？③ 可用什么方法论产出这些材料？
> ④ 材料内容具体有哪些（字段级）？⑤ 如何版本化、迭代与管理？
>
> **权威来源约定（用户硬约束）**：本文所有 SDIE 事实仅取自当前工作空间
> `SDIE-RACI-Matrix.md`（7 人类 / 12 Agent 治理版，唯一权威明细）与 `SDIE-Analysis.md`。
> 方法论与外部实践通过权威渠道取得并标注出处。

---

## 0. 一图速览：五问与本文对应

| 你的问题 | 本文回答章节 |
|----------|--------------|
| ① Implement 应执行哪些工作？ | §2 阶段定位与 Gate 3、§3 递进工作清单（三 A 责任链） |
| ② 分别应输出哪些材料？ | §4 输出材料清单 |
| ③ 可用什么方法论产出这些材料？ | §5 方法论映射（空间已定义 vs 权威推荐） |
| ④ 材料内容具体有哪些？ | §6 各材料字段级示例 |
| ⑤ 如何版本化、迭代与管理？ | §7 版本化 / 迭代 / 变更管理 |

---

## 1. SDIE 与 Implement 阶段定位

SDIE = **S**pec / **D**esign / **I**mplement / **E**valuation，是面向 AI Coding 的治理框架。
其关键改造（见 `SDIE-RACI-Matrix.md` §1）：**A（问责 / 批准权）永远由人类承担；AI Agent 仅作为 R（执行者）**。

四阶段中，Implement 是**最复杂、问责最重**的阶段——由 Coding Agent 主导编码，人类在**三层把关**下逐步放行。
它呈现 SDIE 独有的 **"三 A" 结构**（见 Gate 3）：QA（④·验证）→ Reviewer（④·签字）→ PO（⑦），三者维度独立、不重复。

### Gate 3：Implement → Evaluation 的准入门禁（§6）

| 项目 | 内容 |
|------|------|
| 准入标准 | Lint 0 违规、测试 100% 通过、安全 0 高危、Case Delta 无未授权删/禁用、PR 标签正确、**Review 通过** |
| 人类审批人（A） | 自动化 + **QA（④·验证）** → **Reviewer（④·签字）** → **PO（⑦ 收货合并）**，按发生顺序串联 |
| 不可委托项 | ④ 代码正确性问责（验证+签字）、⑤ 安全判定、⑥ 测试删/禁用授权、⑦ 收货合并、⑨ 越级拦截 |

> 关键纪律（§6 末注）：自动化门禁只能证明"格式与约束合规"，**不能证明"代码正确"**，所以 ④·验证 与 ④·签字 必须是人。
> 三者维度独立：**证据可信（QA）→ 代码正确（Reviewer）→ 业务接受（PO）**。

### Gate 3 准入检查清单（三 A 串联签批前逐项核对）

> 本清单是 `SDIE-RACI-Matrix.md:347` Gate 3 准入标准（**Lint 0 违规、测试 100% 通过、安全 0 高危、Case Delta 无未授权删/禁用、PR 标签正确、Review 通过**）的可执行化。须按 **QA（④·验证）→ Reviewer（④·签字）→ PO（⑦）** 顺序串联，三者维度独立、不得合并或跳步。

| # | 检查项 | 对应产出物 / 依据 | 不可委托红线 |
|---|--------|------------------|--------------|
| 1 | **Lint / 构建 0 违规**：build & lint 零告警，提交带 Conventional Commit trailer | `PR-template.md` §6.1 | — |
| 2 | **测试 100% 通过，覆盖率/变异达标**：单测全绿，覆盖率与变异分（PITest）达阈值（③ 共定值） | 测试报告；§6.3 | ④·验证（QA） |
| 3 | **安全 0 高危**：安全扫描零高危，无越权/注入；安全判定由安全/红队出具 | 安全评估；§6 | ⑤ 安全判定 |
| 4 | **Case Delta 无未授权删/禁用**：测试删/禁用均带 `Test-Disable-Authorization` trailer（⑥），否则 Gate 3 卡住 | Case Delta 报告；§6.5 | ⑥ 测试删/禁用授权 |
| 5 | **PR 标签 / 证据链正确**：PR 标签、关联 TASK/DECOMP 正确，上下文注入合规 | `PR-template.md` | ⑨ 越级拦截（人类在环） |
| 6 | **Review 通过（三 A 串联）**：QA 验证（④·验证）→ Reviewer 签字（④·签字）→ PO 收货（⑦）顺序正确、证据齐备 | Review 清单/测试报告/PR；§3 | ④·验证 / ④·签字 / ⑦ 收货 |

> **放行动作**：三 A 串联（QA 验证 → Reviewer 签字 → PO 收货）全部 ✅ 后，于 PR 描述签 **Gate 3 Approved**，方可进入 Evaluation。任一 ❌ 退回对应 R 修正，**不得带伤过门**。自动化门禁只证明格式合规，不能替代人的正确性判断（④ 红线）。

---

## 2. Implement 阶段 RACI 速查（引自 §3.3）

| 角色 | RACI | 本阶段定位 |
|------|:----:|-----------|
| 产品经理 PM/PO | **A** | **收货合并决策（⑦）**：在 Reviewer 技术批准（④·签字）后，确认是否达验收标准、接不接受进主干 |
| 业务专家 SME | **I** | 知会实现中对验收语义的落地，异常时介入 |
| Tech Lead / 架构负责人 | **R** | 技术领导、审查 Agent 输出、Gate 3 技术侧把关；不再独揽 A（与 Reviewer 共保质量） |
| 开发工程师 Task Owner | **R** | 向 Coding Agent 注入上下文；审查 Agent 输出；本地自验；产出 Review 记录、自验结果。**对 Agent 输出判定不可委托（⑨）** |
| 测试架构师 / QA | **A** | **正确性验证（④·验证）**：对测试证据有效性负最终责——测什么层级、覆盖率、**变异分(PITest)是否达标**、用例集有无被篡改（Case Delta）；建立质量门禁 |
| 安全 / 红队 | **C** | 维护扫描规则、风险评估；**策略判定与漏洞定级不可委托（⑤）** |
| 评审官 Reviewer | **A** | 对 AI 生成代码/测试/数据做**技术批准签字（④·签字）**，承担最终质量责任（Gate 3 "Review 通过"） |

> Agent 侧：Coding Agent ●（落位 `src/` + PR）、Test Designer/Generator/Runner ◐（落位 `docs/specs/task-specs/*.yaml`、`tests/`、CI 产物）、Review Agent ◐（PR 评论，**不代签字**）；所有产出需人类复核（§4）。

---

## 3. Implement 应执行的递进工作（动作级清单，引自 §3.5.3）

行序＝本阶段**递进执行顺序**，即 Gate 3 正式责任链：开发写 → 技术/安全支撑 → QA 验证(④·验证) → Reviewer 签字(④·签字) → PO 收货(⑦) → 知会。

| # | 角色 | RACI | 具体工作项 |
|---|------|:----:|-----------|
| 1 | 开发工程师 (Task Owner) | **R** | 注入上下文（ADR / 相关代码）；审 Agent 输出（判定不可委托 ⑨）；本地自验（build / test / lint）；处理 Agent 报错与重试 |
| 2 | Tech Lead / 架构负责人 | **R** | 技术领导与疑难攻关；审 Agent 输出架构符合性；Gate 3 技术侧把关 |
| 3 | 安全 / 红队 | **C** | 维护扫描规则（`.sdie/security-rules.yaml`）；评估 AI 代码风险；漏洞定级（不可委托 ⑤） |
| 4 | 测试架构师 / QA | **A** | 建立正确性证据：设计测试策略与分级、设定门禁阈值（③ 与 TL 共定）；监控覆盖率 / 变异(PITest)趋势并判定是否达标；审 Case Delta 报告有无未授权删/禁用；对"测试证据有效性"担最终责（④·验证）；**不代 Reviewer 做技术批准（④·签字）** |
| 5 | 评审官 (Reviewer) | **A** | 执行代码 Review（正确性 / 安全 / 规范）；在 QA 证据（④·验证）基础上做**技术批准签字**（「本人承担最终质量责任」）；Gate 3 "Review 通过"（不可委托 ④·签字） |
| 6 | 产品经理 (PM/PO) | **A** | 审 PR 是否达到验收标准；在 Reviewer 技术批准之后做**收货合并决策**（⑦）；对「意图漂移」做最终拦截 |
| 7 | 业务专家 (SME) | **I** | 知会；验收语义漂移时介入澄清 |

**核心不可委托红线（§5）**：
- **④ 代码正确性问责**：验证测试证据（QA）与技术批准签字（Reviewer）均不可委托给 Agent；Review Agent 输出只是建议，不得代签字。
- **⑤ 安全策略判定与合规结论**归安全/红队，Agent 不得改安全配置、不得对生产跑模糊测试。
- **⑥ 测试删除/禁用的显式授权**：必须由提交 commit trailer 的人类显式授权（见 §6.1）。
- **⑦ 收货合并决策**归 PO，发生在 Gate 3 末段。
- **⑨ Agent 行为越级时的拦截**（Ask First 未确认就执行）由任意人类兜底。

---

## 4. 输出材料清单

Implement 阶段的产出物（引自 §3.3 / §3.5.3 / §4）共六类，均需在正文第 1 节「元信息」挂七字段
（id / title / status / phase / owner / related_docs / last_updated，见 §7）：

> 载体格式遵循**全局模板格式选型原则**（`SDIE-Analysis.md` §8.1）：人类契约型（Review 建议、Case Delta 报告）用 Markdown、第 1 节「元信息」七字段；机器规格型（行为清单、测试代码、Harness 配置）用 YAML、`meta:` 块下挂七字段，其余业务字段归入分组键（如 `checklist:`）。

| 材料 | 主要作者（R） | 问责者（A） | 关键读者（C/I） | 落位（参考） |
|------|--------------|------------|----------------|-------------|
| **实现代码 + 自验结果** | Coding Agent（R） | PO（⑦）/ QA（④·验证） | Reviewer（④·签字）、Dev(R)、Tech Lead(R) | `src/` + PR |
| **行为清单 Behavior Checklist** | Test Designer（◐） | QA / Reviewer | Dev(R)、Coding Agent | `docs/specs/task-specs/*.yaml` |
| **测试代码** | Test Generator（◐） | QA / Reviewer | Dev(R) | `tests/` |
| **测试报告 / 覆盖率** | Test Runner（◐） | QA（④·验证） | Reviewer、PO | CI 产物 |
| **Review 建议（PR 评论）** | Review Agent（◐） | Reviewer（④·签字） | Dev(R) | PR 评论（不代签字） |
| **Case Delta 报告**（测试删/禁用审计） | Dev / 安全（R） | 提交 trailer 的人类（⑥） | QA（④·验证） | 随 PR 提交 |

> 注：Gate 3 把"代码正确"拆成多份独立证据——行为清单定义"验什么"、测试代码+报告证明"验过了"、
> Case Delta 证明"没偷偷删测试"、Review 建议在人类签字前提供线索。它们共同构成 ④·验证 的证据链。

---

## 5. 可用方法论映射（空间已定义 vs 权威推荐）

> 标记说明：**【空间已定义】**＝SDIE 工作空间内已规定；**【权威推荐】**＝经权威渠道取得的外部实践，作为补充方法论。

| 产出物 / 工作 | 推荐方法论 | 来源性质 | 权威出处 |
|---------------|-----------|----------|----------|
| 实现 + 自验纪律 | **TDD（测试驱动开发）** | 【权威推荐】 | Kent Beck，*Test-Driven Development: By Example* (2003) |
| 测试组织 / 分级 | **Test Pyramid**（单元/集成/端到端） | 【权威推荐】 | Mike Cohn；martinfowler.com/articles/practical-test-pyramid |
| ④·验证 核心证据：变异分 | **Mutation Testing / PITest** | 【权威推荐】 | Henry Coles，pitest.org |
| ⑥ 测试删/禁用授权 | 【空间已定义】commit trailer 显式授权 | 【空间已定义】 | `SDIE-RACI-Matrix.md` §5⑥ |
| PR 提交规范 | **Conventional Commits** | 【权威推荐】 | conventionalcommits.org |
| 分支 / 合并工作流 | **Trunk-based Development / Git Flow** | 【权威推荐】 | trunkbaseddevelopment.com |
| ④·签字 Review 清单 | **Google 代码审查指南** | 【权威推荐】 | Google，google.github.io/eng-practices/review |
| 版本化 / 迭代 / 变更 | **SemVer 2.0.0** + **ADR** | 【权威推荐】 | semver.org；Michael Nygard 2011 |

**原则**：空间已定义的（⑥ commit trailer 授权、④ 验证/签字 human-only、⑤ 安全判定 human-only、⑨ 越级拦截）必须执行；
缺口的方法论以权威推荐形式补足，团队可裁剪但需记录在 ADR 中。

---

## 6. 各材料具体内容（字段级示例）

### 6.1 PR + Commit Trailer（对应 ⑥ 测试删/禁用授权）

```markdown
## PR：购物车加入商品接口（TASK-CART-001）
### 自验结果
- build: pass | lint: 0 violation | test: 100% pass | 安全扫描: 0 high
### 变更摘要
- 新增 POST /cart/add
- 测试：保留全部 12 条用例（无删/禁用）
### 测试删/禁用声明（⑥）
- 本次无测试删除/禁用
- 若曾删除：`Test-Disable-Authorization: dev-zhang <理由: 该用例依赖已废弃的 mock 服务，见 ADR-xxx>`
```

> 红线：任何测试删除/禁用**必须**带 `Test-Disable-Authorization` trailer 并由人类显式授权（⑥）；QA 在 ④·验证 中审 Case Delta，无 trailer 的删/禁用 = Gate 3 卡住。

### 6.2 行为清单（Behavior Checklist，Test Designer 产出）

```yaml
id: BC-CART-001
title: 加购行为清单
related_docs: [TASK-CART-001, AC-cart.md]
behaviors:
  - id: B1
    given: 库存>0，已登录用户
    when: 点击"加入购物车"
    then: 购物车+1，返回 200
  - id: B2
    given: 库存=0
    when: 点击"加入购物车"
    then: 按钮禁用，返回 409，无写操作
coverage_note: B1/B2 覆盖 AC-1/AC-2；并发超卖见 AC 红卡，留 Implement 处理
```

### 6.3 测试报告 / 覆盖率（Test Runner 产出，④·验证 证据）

```
## Test Report: TASK-CART-001
- 用例数: 12 | 通过: 12 | 失败: 0
- 覆盖率(line): 86% | 分支: 79%
- 变异分(mutation/PITest): 64%   ← ④·验证 核心；低于阈值须补测试
- Case Delta: 0 删除 / 0 禁用（无未授权改动）
```

### 6.4 Review 清单（Reviewer ④·签字 依据）

```
## Review Checklist（Reviewer 签字前逐项确认）
- [ ] 正确性：实现满足 acceptance_criteria（AC-1/AC-2）
- [ ] 安全：无越权/注入；未改安全配置（⑤）
- [ ] 规范：命名/分层符合设计契约
- [ ] 证据：QA 变异分达标、Case Delta 干净（消费 ④·验证 结论）
- [ ] 声明：「本人承担最终质量责任」→ 签字
```

### 6.5 Case Delta 报告（测试删/禁用审计，对应 ⑥）

```
## Case Delta Report
- 删除用例: 0
- 禁用用例: 0
- 未授权改动: 无
- 结论: Gate 3 ③ 项（无未授权删/禁用）通过
```

---

## 7. 版本化 / 迭代 / 变更管理

Implement 材料沿用 Spec/Design 指南 §7 的三层机制（元信息七字段 + 状态机 + SemVer + ADR + Git 闭环），并补充阶段专属门槛。

### 7.1 元信息七字段（所有 Implement 材料第 1 节挂此元数据）
同 Spec 指南 §7.1：`id / title / status / phase / owner / related_docs / last_updated`。

### 7.2 文档/PR 状态机（status 字段取值与流转）
`draft → review → baseline → change → superseded`。
> **基线化（baseline）是硬门槛**：只有 status=baseline 的 Implement 产物（自验通过 + Review 通过 + 证据链完整）才能过 **Gate 3** 进入 Evaluation。

### 7.3 版本号：SemVer 2.0.0（权威推荐，semver.org）
对 Implement 产出/PR 采用 `MAJOR.MINOR.PATCH`：

| 递增 | 触发条件（Implement 语境） |
|------|----------------------|
| **MAJOR** | 不兼容的接口/契约反转：破坏性 API 变更、验收语义变更 |
| **MINOR** | 向后兼容的新增：新增端点、扩展行为 |
| **PATCH** | 向后兼容的修正：bug 修复、阈值微调 |

### 7.4 决策记录：ADR（权威推荐，Michael Nygard 2011）
Implement 中出现的架构/测试策略/安全策略调整若影响基线，写 ADR（不可委托 ② 仍归 Tech Lead；⑤ 安全归安全/红队）。落位 `docs/design/` 或 `docs/adr/`。

### 7.5 基线化变更流程（Implement 迭代闭环）
1. 任何对 baseline 实现的修改请求 → 开 `change` 状态分支，记录动机（挂 ADR）。
2. 按 §3 责任链重新走：Dev 改（R）→ Tech Lead/安全 支撑（R/C）→ QA 验证（④·验证）→ Reviewer 签字（④·签字）→ PO 收货（⑦）。
3. 版本号按 7.3 递增；`last_updated` 刷新；`related_docs` 互链。
4. 重新过 Gate 3（自动化 + 三 A 串联）→ 新 baseline。
5. 旧版本置 `superseded by <new-id>`，保留考古记录，不删除。

### 7.6 Git 约定（权威推荐实践，落地建议）
| 约定 | 规则 |
|------|------|
| 分支 | `feature/<task-id>` 开发分支；合并经 Gate 3（三 A 串联） |
| 提交类型 | `feat:` / `fix:` / `test:` / `refactor:`（Conventional Commits） |
| 标签 | 发布候选打 `vX.Y.Z-rc`；合并打 `vX.Y.Z` |
| 不可委托 | ⑥ 测试删/禁用需 trailer 授权；④·签字 由 Reviewer；⑦ 收货由 PO；⑨ 越级由任意人类拦截 |

---

## 8. 引用与权威来源清单

### 8.1 工作空间（SDIE 事实唯一内部权威）
- `SDIE-RACI-Matrix.md`
  - §1 RACI 基础约定（A 永属人类，Agent 仅 R；多 A 规则 → Gate 3 三问责）
  - §2 概览矩阵（Implement 行：PM=A⑦、QA=A④·验证、Reviewer=A④·签字、TL/Dev=R、安全=C）
  - §2.1 各角色说明（④·验证/签字 拆分、⑤ 安全判定、⑦ 收货、⑨ 拦截、⑩ Harness）
  - §3.3 Implement 阶段 RACI
  - §3.5.3 Implement 阶段动作级工作清单
  - §4 AI Agent 执行矩阵（Coding/Test/Review Agent 落位 `src/` `tests/` PR）
  - §5 不可委托清单 ④⑤⑥⑦⑨
  - §6 Gate 3（Implement→Evaluation，三 A 串联）
- `SDIE-Analysis.md`：§3.3 Implement 详解（三 A）、§6 不可委托、§7 Gate 3、§9 误区

### 8.2 外部权威方法论（经权威渠道取得，标注出处）
| 方法论 | 权威来源 |
|--------|----------|
| Test-Driven Development (TDD) | Kent Beck (2003), *Test-Driven Development: By Example* |
| Test Pyramid / Test Strategy | Mike Cohn; martinfowler.com/articles/practical-test-pyramid |
| Mutation Testing / PITest | Henry Coles, pitest.org |
| Conventional Commits | conventionalcommits.org |
| Trunk-based Development | trunkbaseddevelopment.com |
| Google Code Review Practices | google.github.io/eng-practices/review |
| Semantic Versioning 2.0.0 | semver.org |
| Architecture Decision Record (ADR) | Michael Nygard (2011), cognitect.com/blog/2011/11/15/documenting-architecture-decisions |

---

## 9. 五问一句话总结

- **① 工作**：Dev 注入上下文+审 Agent 输出+自验（R，⑨）→ Tech Lead 技术把关（R）→ 安全评估风险（C，⑤）→ QA 建正确性证据（A，④·验证）→ Reviewer 技术批准签字（A，④·签字）→ PO 收货合并（A，⑦）→ SME 知会（I）。
- **② 材料**：实现代码+自验、行为清单、测试代码、测试报告/覆盖率、Review 建议、Case Delta 报告六类，均挂元信息七字段（MD 第 1 节 / YAML `meta:` 块）。
- **③ 方法论**：空间已定义（④ 验证/签字 human-only、⑤ 安全 human-only、⑥ commit trailer 授权、⑨ 越级拦截）；权威推荐补足（TDD / Test Pyramid / Mutation Testing(PITest) / Conventional Commits / Trunk-based / Google Review）。
- **④ 内容**：见 §6 字段级示例——PR+trailer、行为清单、测试报告(含变异分)、Review 清单、Case Delta 报告。
- **⑤ 版本化**：元信息七字段 + status 状态机（baseline 才能过 Gate 3）+ SemVer + ADR + Git 三 A 串联变更闭环。

---

> 本文由 AI 协作生成，SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；
> 重要决策（尤其 Gate 3 三问责与不可委托项 ④⑤⑥⑦⑨）请由 QA / Reviewer / PO 共同审定。
