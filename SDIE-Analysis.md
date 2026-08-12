---
id: DOC-ANALYSIS-SDIE-001
title: 什么是 SDIE — 详细分析
status: active
related_docs:
  - SDIE-RACI-Matrix.md
last_updated: 2026-08-04
---

# 什么是 SDIE — 详细分析

> 本文基于工作区权威文档 `SDIE-RACI-Matrix.md`（7 人类 / 12 Agent 的 RACI 明细）对 SDIE 做系统拆解。

## 1. 一句话定义

**SDIE 是一套面向 AI Coding（人机协作编程）的工程阶段框架与治理模型**，把软件交付拆成四个阶段：
**S**pec（规格）→ **D**esign（设计）→ **I**mplement（实现）→ **E**valuation（评估）。

它本身不是一个工具或平台，而是一套"**谁在何时、对什么负责**"的纪律——由 RACI 责任矩阵、四道门禁（Gate）、
以及"不可委托清单"共同承载。它的核心问题不是"怎么写代码快"，而是"当代码能被 AI 秒级生成时，
**人类把判断权握在手里**"。

## 2. 词源与定位

- 四个字母 = 四个阶段首字母缩写：**S**pec / **D**esign / **I**mplement / **E**valuation。
- 它是对通用工程生命周期（spec → design → build → verify → review → ship）的**AI Coding 特化**：
  把"构建 / 验证 / 评审 / 发布"压缩进 Implement 与 Evaluation，并用**强人治约束**防止 Agent 越权。
- 在 `SDIE-RACI-Matrix.md` 中，它明确是"AI Coding 场景下的权威 RACI 明细"。

## 3. 四个阶段详解

### 3.1 Spec（规格）
- **目标**：把模糊的业务意图变成**可验证的验收标准**（acceptance_criteria）。
- **问责方 A**：产品经理 PM/PO（A/R 双担，既主导执行又负最终责）。
- **关键产物**：PRD、验收口径清单、`TASK-*.yaml`（Task Spec 骨架）。
- **关键边界**：业务需求与验收语义拍板**不可委托给 Agent（①）**——Agent 只能补草稿，不得自创领域规则。
- **门禁**：Gate 1（Spec→Design），由 PM 审批。

### 3.2 Design（设计）
- **目标**：架构选型、ADR 定稿、上下文注入策略、把 Task Spec 拆成 Agent 可独立完成的原子任务。
- **问责方 A**：Tech Lead / 架构负责人（A/R 双担）。
- **关键产物**：ADR、原子分解方案、上下文注入策略（决定 Agent 能读哪些文件）。
- **关键边界**：**架构选型与 ADR 定稿不可委托（②）**；门禁阈值（覆盖率/变异/安全级别）由 Tech Lead + QA 共定（③）。
- **门禁**：Gate 2（Design→Implement），由 Tech Lead 审批。

### 3.3 Implement（实现）—— 最复杂的阶段
- **目标**：由 Coding Agent 主导编码，人类在**三层把关**下逐步放行。
- **这是 SDIE 的问责重心，呈现"三 A"结构**（见下图 Gate 3）：
  1. **QA（④·验证）**——测试证据是否**有效**？看覆盖率、**变异分(PITest)**、用例集有无被篡改（Case Delta）。
  2. **Reviewer（④·签字）**——代码**对不对**？在 QA 证据之上做技术批准签字。
  3. **PO（⑦）**——**收不收货**？技术批准之后判断是否接受进主干。
- **门禁**：Gate 3（Implement→Evaluation），自动化门禁 + 上述三重人类问责串联。

### 3.4 Evaluation（评估）
- **目标**：质量度量、发布放行、安全对抗演练（提示注入/越狱/越权）、业务价值确认。
- **问责方 A**：测试架构师 / QA 负责人（R/A 双担，发布放行不可委托 ⑧）。
- **关键产物**：质量看板、发布决策与回滚预案、合规结论（⑤ 仍归安全/红队）。
- **注意**：本版（7/12）中 PM 在 Evaluation 降为 C（确认业务价值），**发布放行 A 归 QA**，与旧 5/7 版（PM 为 A）不同。
- **门禁**：Gate 4（Evaluation→交付），由 QA 审批。

## 4. 角色体系：7 人类 + 12 Agent

### 7 类人类角色
1. 产品经理 PM/PO——业务价值与验收标准的 owner（需求锚）。
2. 业务专家 SME——领域语义最终裁判。
3. Tech Lead / 架构负责人——技术方向与架构唯一问责者（技术锚）。
4. 开发工程师 Task Owner——Agent 的"现场 supervisor"，把意图翻译成原子任务。
5. 测试架构师 / QA 负责人——质量门禁与发布决策最终问责者。
6. 安全 / 红队工程师——安全与合规最终判定者。
7. 评审官 Reviewer——AI 代码正确性的**技术批准签字人**（④·签字）。

### 12 类 AI Agent（仅出现在 R / 执行侧）
Spec Agent、Design Agent、Coding Agent、Test Designer、Test Generator、Test Runner、
Review Agent、Failure Analyzer、Self-healing Maintainer、Coverage Analyst、Fuzz/AST Agent、Eval Agent。

**关键约束**：Agent 不得替人类拍板需求/架构、不得改安全配置、不得删/禁用测试、
不得代签字、不得对生产环境跑模糊测试；所有产出需人类复核。

## 5. RACI 的核心改造

| 字母 | 含义 | SDIE 约束 |
|------|------|-----------|
| R 执行 | 实际做事 | 人类或 Agent 均可，Agent 是主要产能 |
| A 问责 | 最终批准/担责 | **永远是人类**；每决策有且只有 1 个 A |
| C 咨询 | 事前征求意见 | 双向沟通，需其专业输入 |
| I 知会 | 事后被告知 | 单向通知 |

SDIE 对 RACI 的关键改造：**A（批准权）严格由人类承担，Agent 本质上是 R（执行者）**。
即使 Agent 是某阶段主导执行者，也不拥有批准权——因为当代码可被秒级重写时，真正有价值的是
"什么是对的"这一判断，必须由人做出。

**多 A 规则**：当某个 Gate 捆绑多个**独立**问责维度时，允许对应人类角色各担其一。
典型即 Gate 3 的三重问责（验证 / 签字 / 收货），三者维度独立、不重复。

## 6. 不可委托清单（10 项）—— SDIE 的底线

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
| ⑨ | Agent 越级时的拦截（Ask First 未确认就执行） | 任意人类兜底 | 全阶段 |
| ⑩ | Harness 本身维护（AGENTS.md / 校验脚本 / Jenkinsfile） | Dev + Tech Lead | 全阶段 |

> 注意：④ 是**一项**不可委托事项（对 AI 是同一条边界），`·验证` / `·签字` 只是它在人类内部的
> 两个归属，不是两条清单项。⑦ 同理是一项。

## 7. 门禁体系（Gate 1–4）

| 门禁 | 准入标准（摘要） | 人类审批人 A |
|------|------------------|--------------|
| Gate 1: Spec→Design | Task Spec 完整、AGENTS.md 最新、验收可测 | PM（QA 为 C 把关验收可测） |
| Gate 2: Design→Implement | 分解合理、上下文策略就绪、Harness 确认 | Tech Lead |
| Gate 3: Implement→Evaluation | Lint 0 违规、测试 100% 通过、安全 0 高危、Case Delta 无未授权删/禁用、PR 标签正确、Review 通过 | QA(④·验证) → Reviewer(④·签字) → PO(⑦) |
| Gate 4: Evaluation→交付 | 质量/效率指标达标、回顾已开、Harness 规则已更新 | QA |

## 8. SDIE 的设计哲学（为什么这样设计）

### 8.1 模板格式选型原则（Markdown vs YAML，全局）

<a id="sec-format-principle"></a>

> **原则（全局，跨四阶段）**：SDIE 所有阶段产出物的载体格式，**由「消费者类型」决定，而非所处阶段**。
>
> - **人类契约型文档（Markdown）**：消费者为 PM / SME / 业务方 / QA / Reviewer 等人类，需可读、可评审、可叙事论证；**第 1 节为「元信息」七字段**（id / title / status / phase / owner / related_docs / last_updated），元数据独立成节、与正文同文呈现。
> - **机器规格型文档（YAML）**：消费者为 Coding Agent / Harness，需程序化解析原子字段、依赖 DAG、Gate 自动校验；**`meta:` 块下挂七字段**（同七字段），其余业务字段归入语义分组键（如 `spec:` / `plan:` / `checklist:` / `metrics:`），与规格主体分离。
>
> 两类互补、不互相外溢：**纯 YAML 仅限机器消费型规格，不应用于人类契约型文档**（如 PRD 转纯 YAML 收益低、牺牲人类对齐价值）。

**四阶段实例映射**（各阶段 §4 产出物均遵循本原则）：

| 阶段 | 人类契约型（→ MD，第 1 节元信息） | 机器规格型（→ YAML，`meta:` 块下挂七字段 + 业务分组键） |
|------|-------------------------------|---------------------|
| Spec | PRD、用户故事地图、`acceptance_criteria` | `TASK-*.yaml` |
| Design | ADR、测试策略、上下文注入策略、安全设计点 | `Decomposition-*.yml` |
| Implement | Review 建议、Case Delta 报告 | 行为清单、测试代码、Harness 配置 |
| Evaluation | 发布决策、质量看板、业务价值确认、回顾 | （多为人类决策文档） |

> 各阶段指南 §4「产出物字段级内容」均引用本全局原则，不再各自重新论证格式选型。

1. **判断权 > 产出权**：当代码可被秒级重写，"什么是对的"才是稀缺价值，必须人做。
2. **自动化门禁的局限**：只能证明"格式与约束合规"，不能证明"代码正确"，所以正确性验证与签字必须是人。
3. **把"验货"拆开**：证据可信（QA）→ 代码正确（Reviewer）→ 业务接受（PO），三问分离，互不替代。
4. **防 Agent 越权**：明确 10 条红线，把 Agent 锁在 R（执行）侧。
5. **反伪证据**：QA 看的是变异分与 Case Delta，而非"覆盖率绿条"；Review 看的是"对不对"而非"格式漂不漂亮"。

## 9. 关键认知与常见误区

- **技术批准 ≠ 收货合并**：Reviewer 的 ④·签字 管"对不对"，PO 的 ⑦ 管"要不要"。
- **覆盖率数字 ≠ 质量证据**：不看变异分（PITest）和用例篡改（Case Delta），绿条可能是弱断言堆出来的。
- **Review Agent 不能代签字**：④ 是红线，它的输出只是建议。
- **架构可行性 ≠ 架构选型**：Tech Lead 在 Spec 只能做可行性初判，选型与 ADR 定稿留到 Design。
- **Agent 输出不是真理**：Task Owner 必须本地自验、批判性审查，不得直接合并。

## 10. 一句话总结

SDIE 是一套"**用四阶段组织 AI 编码、用 RACI + 门禁 + 不可委托清单把人类问责钉死**"的治理框架——
它不追求让 AI 替人拍板，而是让 AI 当高效执行者、让人守住"什么是对的"这条底线。
