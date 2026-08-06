---
id: DOC-EVALUATION-GUIDE-001
title: SDIE Evaluation 阶段工作指南（工作项 / 产出物 / 方法论 / 字段级内容 / 版本化管理）
status: draft
phase: Evaluation
owner: 测试架构师 / QA 负责人
related_docs:
  - SDIE-RACI-Matrix.md
  - SDIE-Analysis.md
  - SDIE-Spec-Guide.md
  - SDIE-Design-Guide.md
  - SDIE-Implement-Guide.md
last_updated: 2026-08-06
---

# SDIE Evaluation 阶段工作指南

> 本文是 SDIE 工程方法在 **Evaluation（评估）阶段** 的操作手册，系统回答五个问题：
> ① Evaluation 应执行哪些工作？② 分别应输出哪些材料？③ 可用什么方法论产出这些材料？
> ④ 材料内容具体有哪些（字段级）？⑤ 如何版本化、迭代与管理？
>
> **权威来源约定（用户硬约束）**：本文所有 SDIE 事实仅取自当前工作空间
> `SDIE-RACI-Matrix.md`（7 人类 / 12 Agent 治理版，唯一权威明细）与 `SDIE-Analysis.md`。
> 方法论与外部实践通过权威渠道取得并标注出处。

---

## 0. 一图速览：五问与本文对应

| 你的问题 | 本文回答章节 |
|----------|--------------|
| ① Evaluation 应执行哪些工作？ | §2 阶段定位与 Gate 4、§3 递进工作清单（R/A/C/I） |
| ② 分别应输出哪些材料？ | §4 输出材料清单 |
| ③ 可用什么方法论产出这些材料？ | §5 方法论映射（空间已定义 vs 权威推荐） |
| ④ 材料内容具体有哪些？ | §6 各材料字段级示例 |
| ⑤ 如何版本化、迭代与管理？ | §7 版本化 / 迭代 / 变更管理 |

---

## 1. SDIE 与 Evaluation 阶段定位

SDIE = **S**pec / **D**esign / **I**mplement / **E**valuation，是面向 AI Coding 的治理框架。
其关键改造（见 `SDIE-RACI-Matrix.md` §1）：**A（问责 / 批准权）永远由人类承担；AI Agent 仅作为 R（执行者）**。

四阶段中，Evaluation 是**质量与价值闭环**：它做质量度量、发布放行、安全对抗演练（提示注入/越狱/越权）、
业务价值确认，并通过 **Gate 4** 交付。本阶段 **A 归 QA**（发布放行不可委托 ⑧）。

> **关键改动（7/12 版）**：在 Evaluation 中，**PM 降为 C**（仅确认业务价值、签"业务价值确认"，非技术放行 A）；
> **发布放行的 A 归属 QA**（不可委托 ⑧）。这与旧 5/7 版（PM 为 A 做业务验收）不同——若沿用旧约定需团队切换并知会成本
> （见 `SDIE-RACI-Matrix.md` §7、§2.1 QA 说明）。

### Gate 4：Evaluation → 交付的准入门禁（§6）

| 项目 | 内容 |
|------|------|
| 准入标准 | 质量/效率指标达标、回顾已开、Harness 规则已更新 |
| 人类审批人（A） | **QA / 测试架构师**（发布放行，⑧） |
| 不可委托项 | ⑧ 发布决策与回滚预案（QA）；⑤ 安全判定（安全/红队）；⑩ Harness 维护（Dev+TL） |

> 关键纪律：Gate 4 的 A 是 QA，不是 PO。PO 的收货合并（⑦）发生在 Gate 3；Evaluation 的发布放行（⑧）是 QA 的独立问责维度。

### Gate 4 准入检查清单（QA 签批前逐项核对）

> 本清单是 `SDIE-RACI-Matrix.md:348` Gate 4 准入标准（**质量/效率指标达标、回顾已开、Harness 规则已更新**）的可执行化。QA 作为 A（发布放行 ⑧）必须**逐项确认通过**后方可签 Gate 4 交付。

| # | 检查项 | 对应产出物 / 依据 | 不可委托红线 |
|---|--------|------------------|--------------|
| 1 | **质量/效率指标达标**：质量看板（DORA+变异分）达标，能力指标（幻觉率/准确率/护栏通过率）达标，发布决策与回滚预案已定 | 质量看板 §6.1 / 发布决策 §6.2 | ⑧ 发布决策（QA） |
| 2 | **安全/合规结论已出**：对抗演练（提示注入/越权/越狱）完成，合规结论由安全/红队出具 | 对抗演练报告 §6.3 | ⑤ 安全判定 |
| 3 | **业务价值确认（PM 降 C）**：PM 仅签"业务价值确认"（C，非技术放行 A） | 业务价值确认 §6.4 | —（PM 已降 C） |
| 4 | **回顾已开**：Retrospective 已记录，Gate 3 三 A 串联等改进项已提炼 | 回顾 §6.6 | — |
| 5 | **Harness 规则已更新**：`AGENTS.md` / 校验脚本 / `Jenkinsfile` 已随评估结论更新（⑩），与最新 RACI 同步 | Harness；§3 | ⑩ Harness 维护（Dev+TL） |

> **放行动作**：QA 在 #1–#5 全部 ✅ 后，于发布分支/PR 描述签 **Gate 4 Approved**，方可交付。任一 ❌ 退回对应 R 修正，**不得带伤过门**。注意 A 归 QA（⑧）；PO 的收货合并（⑦）已在 Gate 3 完成，本阶段不重复。

---

## 2. Evaluation 阶段 RACI 速查（引自 §3.4）

| 角色 | RACI | 本阶段定位 |
|------|:----:|-----------|
| 产品经理 PM/PO | **C** | 确认交付物达成业务价值，签署"业务价值确认"（配合 QA 发布决策；非技术放行 A） |
| 业务专家 SME | **I** | 知会验收结论，确认领域边界未被破坏 |
| Tech Lead / 架构负责人 | **C** | 知会评估结论；Harness 更新监督（⑩），不担 Evaluation 的 A（A 归 QA） |
| 开发工程师 Task Owner | **I** | 知会评估结论，按需更新 AGENTS.md / 校验脚本（Harness 维护不可委托 ⑩） |
| 测试架构师 / QA | **R/A** | **R**：质量度量、发布决策执行、质量看板；**A**：发布放行担责（不可委托 ⑧） |
| 安全 / 红队 | **C** | 对抗演练（提示注入/越狱/越权）、合规结论（不可委托 ⑤） |
| 评审官 Reviewer | **C** | 参与回顾，反馈 Agent 输出质量问题，推动 Harness 规则迭代 |

> Agent 侧：Failure Analyzer ●（根因/flake 报告）、Self-healing Maintainer ◐（修复后测试，待人复核）、
> Coverage Analyst ◐（待补测试建议）、Fuzz/AST Agent ◐（崩溃报告 `tests/fuzz/`）、Eval Agent ●（能力指标，落位 `docs/eval/`）；
> 所有产出需人类复核（§4）。

---

## 3. Evaluation 应执行的递进工作（动作级清单，引自 §3.5.4）

行序＝本阶段**递进执行顺序**（主导度量 → 对抗/价值确认 → 知会）。

| # | 角色 | RACI | 具体工作项 |
|---|------|:----:|-----------|
| 1 | 测试架构师 / QA | **R/A** | 执行质量度量（覆盖率 / 变异 / 缺陷率）；主导发布决策与回滚预案（不可委托 ⑧；**回滚的质量触发条件与回滚验收标准由 QA 定，执行机制与 Tech Lead（及组织 SRE，若有）共担**）；维护质量看板 |
| 2 | 安全 / 红队 | **C** | 对抗演练（提示注入 / 越狱 / 越权）；出具合规结论（不可委托 ⑤） |
| 3 | 产品经理 (PM/PO) | **C** | 确认交付达成业务价值；签「业务价值确认」（非技术放行） |
| 4 | 评审官 (Reviewer) | **C** | 参与回顾；反馈 Agent 质量问题，推动 Harness 规则迭代 |
| 5 | Tech Lead / 架构负责人 | **C** | 知会评估结论；监督 Harness 更新（不可委托 ⑩） |
| 6 | 开发工程师 (Task Owner) | **I** | 知会评估结论；按需更新 AGENTS.md / 校验脚本（Harness 维护不可委托 ⑩） |
| 7 | 业务专家 (SME) | **I** | 知会验收结论；确认领域边界未被破坏 |

**核心不可委托红线（§5）**：
- **⑧ 发布决策与回滚预案**必须由 QA 担责（A）；回滚的质量触发条件与验收标准由 QA 定，执行机制与 Tech Lead（及 SRE）共担。
- **⑤ 安全策略判定与合规结论**归安全/红队，Agent 不得对生产跑模糊测试、不得改安全配置。
- **⑩ Harness 维护**（AGENTS.md / 校验脚本 / Jenkinsfile）由 Dev + Tech Lead 负责，非 Agent 自改。

---

## 4. 输出材料清单

Evaluation 阶段的产出物（引自 §3.4 / §3.5.4 / §4）共七类，均需在 frontmatter 挂七字段
（id / title / status / phase / owner / related_docs / last_updated，见 §7）：

| 材料 | 主要作者（R） | 问责者（A） | 关键读者（C/I） | 落位（参考） |
|------|--------------|------------|----------------|-------------|
| **质量看板 / 度量报告** | QA（R） | QA（A，⑧） | PM(C)、Tech Lead(C)、Reviewer(C) | 看板 / `docs/eval/` |
| **发布决策 + 回滚预案** | QA（R/A，⑧） | QA（A） | Tech Lead(C，执行机制共担)、PO(C) | `docs/eval/release-decision.md` |
| **对抗演练报告** | 安全 / 红队（C） | 安全/红队（⑤） | QA(C)、Tech Lead(C) | `docs/eval/red-team.md` |
| **合规结论** | 安全 / 红队（C） | 安全/红队（⑤） | QA(C)、PO(C) | `docs/eval/compliance.md` |
| **业务价值确认** | PM/PO（C） | PM/PO（签确认，非 A） | QA(A)、SME(I) | `docs/eval/business-value.md` |
| **能力指标报告（Eval Agent）** | Eval Agent（●） | QA / Tech Lead（复核） | Reviewer(C) | `docs/eval/` |
| **回顾 / Retrospective** | Reviewer(C) 牵头 | 团队（知会） | 全员 I/C | `docs/eval/retro.md` |

> 注：Evaluation 材料是**价值闭环的多维证据**——质量看板证"稳不稳"、发布决策证"放不放"、
> 对抗演练证"抗不抗打"、业务价值确认证"值不值"、回顾证"能不能更好"。它们通过 `related_docs` 互链成交付包。

---

## 5. 可用方法论映射（空间已定义 vs 权威推荐）

> 标记说明：**【空间已定义】**＝SDIE 工作空间内已规定；**【权威推荐】**＝经权威渠道取得的外部实践，作为补充方法论。

| 产出物 / 工作 | 推荐方法论 | 来源性质 | 权威出处 |
|---------------|-----------|----------|----------|
| 质量度量 / 效率指标 | **DORA Metrics**（部署频率/交付前置时间/变更失败率/MTTR） | 【权威推荐】 | Forsgren, Humble, Kim，*Accelerate* (2018)；dora.dev |
| ④·验证 延续：变异分 | **Mutation Testing / PITest** | 【权威推荐】 | Henry Coles，pitest.org |
| 发布决策与回滚预案（⑧） | 【空间已定义】QA 定质量触发条件与回滚验收标准 | 【空间已定义】 | `SDIE-RACI-Matrix.md` §3.4/§5⑧ |
| 韧性 / 对抗演练（⑤） | **Chaos Engineering**（原则） | 【权威推荐】 | principlesofchaos.org（Netflix 起源） |
| 安全对抗（⑤） | **Red Teaming** 框架（提示注入/越狱/越权） | 【权威推荐】 | 安全红队通用实践；NIST/行业红队方法 |
| 回顾 / Retrospective | **Postmortem / Retrospective**（无责复盘） | 【权威推荐】 | Google SRE Book，sre.google/sre-book；Norm Kerth *Project Retrospectives* |
| 能力指标（Eval Agent） | **LLM Capability Eval**（幻觉率/准确率/护栏通过率） | 【权威推荐】 | 大模型评测通用实践；落位 `docs/eval/` |
| Harness 更新（⑩） | 【空间已定义】Dev+TL 维护 AGENTS.md/校验脚本 | 【空间已定义】 | `SDIE-RACI-Matrix.md` §5⑩ |
| 版本化 / 迭代 / 变更 | **SemVer 2.0.0** + **ADR** | 【权威推荐】 | semver.org；Michael Nygard 2011 |

**原则**：空间已定义的（⑧ 发布决策 QA 担、⑤ 安全判定 human-only、⑩ Harness human-only、PM 在 Eval 降 C）必须执行；
缺口的方法论以权威推荐形式补足，团队可裁剪但需记录在 ADR 中。

---

## 6. 各材料具体内容（字段级示例）

### 6.1 质量看板 / 度量报告（Quality Dashboard）

```
## Quality Dashboard: <项目>-<迭代>
- 覆盖率(line): 86% | 分支: 79% | 变异分(mutation): 64%
- 缺陷率: 0.4/kloc | 逃逸缺陷: 1
- DORA: 部署频率=每日 | 前置时间=2.1天 | 变更失败率=8% | MTTR=45min
- 趋势: 变异分较上迭代 +6pt（关注点：订单模块）
```

### 6.2 发布决策 + 回滚预案（对应 ⑧）

```
## Release Decision
- 决策: 放行（Go / No-Go）
- 质量触发条件(QA 定): 变异分≥60% 且 0 高危且 缺陷率<1/kloc
- 回滚验收标准(QA 定): 5 分钟内恢复至 vX.Y.(Z-1)，核心链路可用率≥99.9%
- 执行机制: Tech Lead + SRE 共担（蓝绿/灰度）
- 不可委托: 本决策 A 归 QA（⑧）
```

### 6.3 对抗演练报告（对应 ⑤）

```
## Red-Team Report
- 场景: 提示注入篡改加购数量 / 越权读他人购物车 / 越狱绕过护栏
- 结果: 注入拦截率 92%；越权 1 例未拦（已修）
- 合规结论: 通过（附整改项 ADR-xxx）
- 不可委托: 判定与结论归安全/红队（⑤）
```

### 6.4 业务价值确认（PM/PO，C 非 A）

```
## Business Value Confirmation
- 目标达成: 购物车放弃率 68%→57%（目标 55%，接近）
- 结论: 达成业务价值；非技术放行（放行 A 归 QA）
- 签署: PM/PO
```

### 6.5 能力指标报告（Eval Agent 产出）

```yaml
id: EVAL-CAP-001
title: Agent 能力指标-购物车迭代
metrics:
  hallucination_rate: 0.03      # 幻觉率
  task_accuracy: 0.94           # 任务准确率
  guardrail_pass_rate: 0.97     # 护栏通过率
  coverage_of_ac: 1.0           # 验收标准覆盖
reviewed_by: QA / Tech Lead
```

### 6.6 回顾 / Retrospective

```
## Retrospective
- 做得好: Gate 3 三 A 串联顺畅，Case Delta 零违规
- 待改进: 并发超卖边界在 Implement 暴露晚，应在 Design 预埋
- 行动项: 更新 AGENTS.md 上下文策略（⑩）；新增并发测试模板（ADR-yyy）
```

---

## 7. 版本化 / 迭代 / 变更管理

Evaluation 材料沿用 Spec/Design/Implement 指南 §7 的三层机制（frontmatter 七字段 + 状态机 + SemVer + ADR + Git 闭环），并补充阶段专属门槛。

### 7.1 frontmatter 七字段（所有 Evaluation 材料挂此元数据）
同 Spec 指南 §7.1：`id / title / status / phase / owner / related_docs / last_updated`。

### 7.2 文档状态机（status 字段取值与流转）
`draft → review → baseline → change → superseded`。
> **基线化（baseline）是硬门槛**：只有 status=baseline 的 Evaluation 包（指标达标 + 发布决策定 + 回顾开 + Harness 更新）才能过 **Gate 4** 交付。

### 7.3 版本号：SemVer 2.0.0（权威推荐，semver.org）
对 Evaluation 产出/发布采用 `MAJOR.MINOR.PATCH`：

| 递增 | 触发条件（Evaluation 语境） |
|------|----------------------|
| **MAJOR** | 不兼容的发布：回滚预案结构反转、质量门禁重大调整 |
| **MINOR** | 向后兼容的新增：新增能力指标维度、扩展对抗场景 |
| **PATCH** | 向后兼容的修正：指标口径澄清、报告措辞修正 |

### 7.4 决策记录：ADR（权威推荐，Michael Nygard 2011）
Evaluation 中出现的发布策略/回滚标准/Harness 调整若影响基线，写 ADR。
**⑧ 发布决策归 QA、⑤ 安全归安全/红队、⑩ Harness 归 Dev+TL**（均不可委托）。落位 `docs/eval/` 或 `docs/adr/`。

### 7.5 基线化变更流程（Evaluation 迭代闭环）
1. 任何对 baseline 评估包的修改请求 → 开 `change` 状态副本，记录动机（挂 ADR）。
2. 按 §3 递进流程重新走：QA 度量/决策（R/A，⑧）→ 安全对抗（C，⑤）→ PM 价值确认（C）→ 回顾（C）→ Harness 更新（⑩）。
3. 版本号按 7.3 递增；`last_updated` 刷新；`related_docs` 互链。
4. 重新过 Gate 4（QA 审批）→ 新 baseline。
5. 旧版本置 `superseded by <new-id>`，保留考古记录，不删除。

### 7.6 Git 约定（权威推荐实践，落地建议）
| 约定 | 规则 |
|------|------|
| 分支 | `release/<version>` 发布分支；经 Gate 4 交付 |
| 提交类型 | `eval:`（度量/报告）、`release:`（发布决策）、`adr:`（决策）、`chore:`（Harness 更新 ⑩） |
| 标签 | 发布打 `vX.Y.Z` |
| 不可委托 | ⑧ 发布决策 QA；⑤ 安全判定安全/红队；⑩ Harness Dev+TL；PM 在 Eval 仅 C |

---

## 8. 引用与权威来源清单

### 8.1 工作空间（SDIE 事实唯一内部权威）
- `SDIE-RACI-Matrix.md`
  - §1 RACI 基础约定（A 永属人类，Agent 仅 R）
  - §2 概览矩阵（Evaluation 行：QA=R/A，PM=C，安全=C，Reviewer=C，TL=C，Dev=I，SME=I）
  - §2.1 各角色说明（⑧ 发布 QA、⑤ 安全判定、⑦ 收货在 Gate3、⑩ Harness）
  - §3.4 Evaluation 阶段 RACI
  - §3.5.4 Evaluation 阶段动作级工作清单
  - §4 AI Agent 执行矩阵（Failure Analyzer/Self-healing/Coverage/Fuzz/Eval Agent 落位看板/`tests/fuzz/`/`docs/eval/`）
  - §5 不可委托清单 ⑤⑧⑩
  - §6 Gate 4（Evaluation→交付，QA 审批）
  - §7 与旧版差异（PM 在 Eval 降 C、发布 A 归 QA）
- `SDIE-Analysis.md`：§3.4 Evaluation 详解、§6 不可委托、§7 Gate 4、§9 误区

### 8.2 外部权威方法论（经权威渠道取得，标注出处）
| 方法论 | 权威来源 |
|--------|----------|
| DORA Metrics / Accelerate | Forsgren, Humble, Kim (2018), *Accelerate*; dora.dev |
| Mutation Testing / PITest | Henry Coles, pitest.org |
| Chaos Engineering | principlesofchaos.org |
| Red Teaming（安全对抗） | 行业红队通用方法；NIST 红队指南 |
| Postmortem / Retrospective | Google SRE Book (sre.google/sre-book); Norm Kerth, *Project Retrospectives* |
| LLM Capability Eval | 大模型评测通用实践；落位 `docs/eval/` |
| Semantic Versioning 2.0.0 | semver.org |
| Architecture Decision Record (ADR) | Michael Nygard (2011), cognitect.com/blog/2011/11/15/documenting-architecture-decisions |

---

## 9. 五问一句话总结

- **① 工作**：QA 主导质量度量+发布决策与回滚预案（R/A，⑧）→ 安全对抗演练与合规结论（C，⑤）→ PM 业务价值确认（C，非 A）→ Reviewer 回顾推动 Harness 迭代（C）→ Tech Lead 监督 Harness 更新（C，⑩）→ Dev/SME 知会（I）。
- **② 材料**：质量看板、发布决策+回滚预案、对抗演练报告、合规结论、业务价值确认、能力指标报告、回顾七类，均挂 frontmatter 七字段。
- **③ 方法论**：空间已定义（⑧ 发布 QA 担、⑤ 安全 human-only、⑩ Harness human-only、PM 降 C）；权威推荐补足（DORA / Mutation Testing / Chaos Engineering / Red Teaming / Postmortem / LLM Eval）。
- **④ 内容**：见 §6 字段级示例——质量看板(DORA+变异分)、发布决策(质量触发+回滚标准)、对抗报告、业务价值确认、能力指标 yaml、回顾。
- **⑤ 版本化**：frontmatter 七字段 + status 状态机（baseline 才能过 Gate 4）+ SemVer + ADR + Git 发布闭环（⑧ 发布 A 归 QA）。

---

> 本文由 AI 协作生成，SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；
> 重要决策（尤其 Gate 4 审批、不可委托项 ⑧⑤⑩、PM 在 Eval 降 C）请由 QA / 安全 / Tech Lead 共同审定。
