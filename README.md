# SDIE 知识库（sdie-knowledge-base）

> 一套面向 **AI Coding（人机协作编程）** 的工程阶段框架与治理模型。
> 核心命题不是"怎么把代码写得更快"，而是"当代码能被 AI 秒级生成时，**人类把判断权握在手里**"。

---

## 一句话定义

**SDIE** = **S**pec（规格）→ **D**esign（设计）→ **I**mplement（实现）→ **E**valuation（评估）。

它把软件交付拆成四个阶段，用 **RACI 责任矩阵 + 四道门禁（Gate）+ 10 项不可委托清单** 共同承载"谁在何时、对什么负责"的纪律：

- **A（问责 / 批准权）永远由人类承担**；AI Agent 仅作为 R（执行者）。
- 即便 Agent 是某阶段的 dominant 执行者，也不拥有批准权——"什么是对的"这一判断必须由人做出。

---

## 目录结构

```
sdie-knowlege-base/
├── overview.md                  # 工程总览（本文的延伸阅读）
├── SDIE-Analysis.md             # 什么是 SDIE — 详细分析（定义/阶段/角色/不可委托/门禁）
├── SDIE-RACI-Matrix.md          # 7 人类 / 12 Agent 的权威 RACI 明细 + 不可委托清单（唯一权威基准）
│
├── SDIE-Spec-Guide.md           # Spec 阶段工作指南（五问：工作/材料/方法论/字段/版本化）
├── SDIE-Design-Guide.md         # Design 阶段工作指南
├── SDIE-Implement-Guide.md      # Implement 阶段工作指南
├── SDIE-Evaluation-Guide.md     # Evaluation 阶段工作指南
│
├── 0-References/                # 权威方法引用库（外部权威方法的知识与溯源，详见其 README）
│
├── 1-Spec/                      # Spec 阶段模板与说明
│   ├── README.md
│   ├── PRD-template.md
│   ├── UserStoryMap-template.md
│   ├── acceptance-criteria-template.md
│   └── TASK-template.yaml
│
├── 2-Design/                    # Design 阶段模板与说明
│   ├── README.md
│   ├── ADR-template.md
│   ├── Decomposition-template.yml
│   ├── Test-Strategy-template.md
│   ├── Context-Injection-template.md
│   └── Security-Design-template.md
│
├── 3-Implement/                 # Implement 阶段模板与说明
│   ├── README.md
│   ├── PR-template.md
│   ├── Behavior-Checklist-template.yaml
│   ├── Case-Delta-template.md
│   ├── Review-Checklist-template.md
│   └── Gate3-Checklist-template.md
│
└── 4-Evaluation/                # Evaluation 阶段模板与说明
    ├── README.md
    ├── Quality-Dashboard-template.md
    ├── Release-Decision-template.md
    ├── Adversarial-Report-template.md
    ├── Business-Value-Confirmation-template.md
    ├── Eval-Metrics-template.yaml
    └── Retrospective-template.md
```

---

## 四个阶段与门禁

| 阶段 | 目标 | 问责方（A） | 门禁 | 审批人 |
|------|------|------------|------|--------|
| **Spec** | 把模糊业务意图收敛为可验证验收标准 | PM/PO（① 不可委托） | Gate 1：Task Spec 完整 / AGENTS.md 最新 / 验收可测 | PM（QA 为 C 把关验收可测） |
| **Design** | 架构选型、ADR 定稿、上下文注入策略、原子任务分解 | Tech Lead（② 不可委托） | Gate 2：分解合理 / 上下文策略就绪 / Harness 确认 | Tech Lead |
| **Implement** | Coding Agent 主导编码，人类三层把关放行 | **三 A 共担**（④·验证 QA → ④·签字 Reviewer → ⑦ PO） | Gate 3：Lint 0 / 测试 100% / 安全 0 高危 / Case Delta 干净 / Review 通过 | QA → Reviewer → PO 串联 |
| **Evaluation** | 质量度量、发布放行、安全对抗演练、业务价值确认 | QA（⑧ 不可委托；PM 在本阶段降为 C） | Gate 4：质量/效率指标达标 / 回顾已开 / Harness 已更新 | QA |

> **Gate 3 三 A 串联**是 AI Coding 中最关键的人类责任落点：证据可信（QA）→ 代码正确（Reviewer）→ 业务接受（PO），三者维度独立、不重复、不得跳步。

---

## 角色体系：7 人类 + 12 Agent

**7 类人类角色（含问责 A）**：产品经理 PM/PO、业务专家 SME、Tech Lead / 架构负责人、开发工程师 Task Owner、测试架构师 / QA、安全 / 红队、评审官 Reviewer。

**12 类 AI Agent（仅执行侧 R）**：Spec Agent、Design Agent、Coding Agent、Test Designer、Test Generator、Test Runner、Review Agent、Failure Analyzer、Self-healing Maintainer、Coverage Analyst、Fuzz/AST Agent、Eval Agent。

> Agent 不得替人类拍板需求/架构、不得改安全配置、不得删/禁用测试、不得代签字、不得对生产环境跑模糊测试；所有产出需人类复核。

---

## 不可委托清单（10 项）—— SDIE 的底线

| # | 不可委托事项 | 归属角色 | 阶段 |
|---|--------------|----------|------|
| ① | 业务需求与验收语义拍板 | PM / SME | Spec |
| ② | 架构选型与 ADR 定稿 | Tech Lead | Design |
| ③ | 门禁阈值设定 | Tech Lead + QA | Design/Eval |
| ④ | 代码正确性问责（验证+签字） | QA（验证）+ Reviewer（签字） | Implement |
| ⑤ | 安全策略判定与合规结论 | 安全/红队 | Implement/Eval |
| ⑥ | 测试删除/禁用的显式授权 | 提交 commit trailer 的人类 | Implement |
| ⑦ | 收货合并决策 | PO / 产品经理 | Implement |
| ⑧ | 发布决策与回滚预案 | QA | Evaluation |
| ⑨ | Agent 越级时的拦截 | 任意人类兜底 | 全阶段 |
| ⑩ | Harness 本身维护（AGENTS.md / 校验脚本 / Jenkinsfile） | Dev + Tech Lead | 全阶段 |

---

## 通用约定（贯穿四阶段）

所有阶段产出文档均挂 **frontmatter 七字段**：`id / title / status / phase / owner / related_docs / last_updated`。

**文档状态机**：`draft → review → baseline → change → superseded`
> 基线化（baseline）是硬门槛：只有 baseline 的包才能过对应 Gate 进入下一阶段；baseline 后内容不可原地涂改，需走 change 流程并递增版本。

**版本号**：SemVer 2.0.0（`MAJOR.MINOR.PATCH`），详见各阶段指南 §7。

**Git 约定**：按阶段分分支（`spec/<feature>` / `design/<feature>` / `feature/<task-id>` / `release/<version>`），提交类型带 `spec:` / `design:` / `feat:` / `eval:` 等前缀，基线/发布打 `vX.Y.Z` 标签。

---

## 如何阅读本知识库

1. **先读 `SDIE-Analysis.md`**：建立对 SDIE 的整体认知（定义、阶段、角色、不可委托、门禁）。
2. **再读 `SDIE-RACI-Matrix.md`**：这是所有事实的**唯一权威基准**（7/12 治理版），四份阶段指南均以其为依据。
3. **按阶段查 `SDIE-*-Guide.md`**：每份指南系统回答五问——① 应执行哪些工作？② 输出哪些材料？③ 可用什么方法论？④ 材料字段级内容？⑤ 如何版本化迭代？
4. **落地时翻对应阶段的模板目录**（如 `1-Spec/`、`3-Implement/`）：每个 `*-template.*` 提供可直接填写的字段级模板，`README.md` 给出使用说明。

---

> 本文档为工程知识库索引，由 AI 协作生成。SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；涉及 Gate 审批与不可委托项的重要决策，请由对应人类角色审定。
