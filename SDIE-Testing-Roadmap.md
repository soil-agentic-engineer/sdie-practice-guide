# SDIE 测试工程技术路径图（Technical Roadmap）

> 本文整理 SDIE（Spec / Design / Implement / Evaluation）框架下的**测试工程**建设路径：
> 以工作空间已定义的测试治理为骨架，以外部权威测试方法论为补充，给出可落地的分阶段路线图、度量/门禁阈值与工具链建议。
>
> **权威来源约定**：SDIE 事实仅取自当前工作空间 `SDIE-RACI-Matrix.md`、`SDIE-Analysis.md` 及四阶段 Guide；
> 外部测试方法论经权威渠道取得并标注出处（见 §8）。本文档是与四阶段 Guide 平行的建设视图，不重复其字段级示例。

---

## 0. 一图速览：四阶段测试工程路径

| 阶段 | 测试工程重心 | 核心产出 | 问责（A） | 门禁 | 主导方法论（权威出处） |
|------|-------------|----------|-----------|------|------------------------|
| **Spec** | 验收可测性奠基 | `acceptance_criteria`（Gherkin）、`TASK-*.yaml` | PM/PO（①） | Gate 1 | BDD / Example Mapping（Dan North；Gojko Adzic） |
| **Design** | 测试策略 + 门禁阈值共定 | 测试策略草案、上下文注入、安全设计点 | Tech Lead（②）+ QA（③） | Gate 2 | Test Pyramid（Cohn/Fowler）、风险驱动测试（ISTQB） |
| **Implement** | TDD 分片实现 + 证据链 | 行为清单、测试代码、变异分报告、Case Delta | QA（④·验证）→ Reviewer（④·签字）→ PO（⑦） | Gate 3 | TDD（Beck）、Mutation Testing / PITest、Conventional Commits |
| **Evaluation** | 质量度量 + 发布放行 + 对抗 | 质量看板、发布决策、对抗演练、能力指标 | QA（⑧） | Gate 4 | DORA Metrics（Forsgren et al.）、威胁建模（STRIDE） |

> 测试工程在 SDIE 中不是孤立阶段，而是**贯穿四阶段的证据链**：Spec 定义"验什么"→ Design 规定"怎么验 / 验多严"→ Implement 产出"验过了"的证据 → Evaluation 用度量与对抗证明"稳不稳、放不放"。

---

## 1. 现状盘点：SDIE 已定义的测试治理资产

（均来自工作空间，可直接复用，无需重新设计）

- **角色与红线**：QA = ④·验证（对测试证据有效性负最终责）；Reviewer = ④·签字；PO = ⑦ 收货；安全/红队 = ⑤ 判定权；发布决策 ⑧ 归 QA；测试删/禁用 ⑥ 需人类 commit trailer 授权。
- **门禁**：Gate 3 = Lint 0 / 测试 100% / 安全 0 高危 / Case Delta 干净 / Review 通过；Gate 4 = 质量效率达标 / 回顾已开 / Harness 更新。
- **Design 测试策略模板**（`2-Design/test-strategy.template.md`）：分级占比 70/20/10、覆盖率 ≥80%、变异分 ≥60%、安全 0 高危；阈值由 Tech Lead + QA 共定（不可委托 ③）。
- **Implement 行为清单**（`3-Implement/behavior-checklist.template.yaml`）：以 `given/when/then` + `maps_to: AC-N` 定义"验什么"，禁止自创验收语义。
- **Implement 报告**：测试报告含覆盖率(line/branch) + 变异分(PITest) + Case Delta。
- **Evaluation 质量看板**：DORA 四指标 + 变异分 + 缺陷率（workspace 已引 *Accelerate* 2018 / dora.dev）。
- **风险基座**：`0-References/risk-matrix.md` 提供 `probability×impact`（1–5，low/med/high）量表，与 DECOMP `risk` 字段对齐。

---

## 2. 总体技术路径图（分阶段活动 × 方法 × 度量）

```
Spec ──► Design ──► Implement ──► Evaluation
 │         │          │            │
验收可测   测试策略     TDD实现      质量度量
BDD/例示   阈值共定     变异达标      发布放行
AC清单    风险分级     三A串联       对抗演练
 │         │          │            │
Gate1      Gate2       Gate3        Gate4
```

### 2.1 各阶段测试工程活动明细

| 阶段 | 测试工程活动 | 输入 | 输出 | 度量/门禁 |
|------|-------------|------|------|-----------|
| Spec | 把需求写成可验证验收；识别并发/边界红卡 | PRD、用户故事地图 | `acceptance_criteria`（Gherkin）、`TASK-*.yaml` | Gate 1：验收可测（QA 为 C 把关） |
| Design | 定测试分级占比；与 TL 共定阈值；定测试数据方案；标安全设计点；风险分级 | `TASK-*.yaml`、ADR | 测试策略草案、上下文注入策略、安全设计点 | Gate 2：阈值共定③、测试策略就绪 |
| Implement | 依行为清单做 TDD；跑单测/集成/变异；审 Case Delta；三 A 串联 | 行为清单、DECOMP、测试策略 | 测试代码、测试报告(变异分)、Case Delta 报告、Review 记录 | Gate 3：测试 100% + 变异分达标 + 0 高危 + Case Delta 干净 |
| Evaluation | 量化质量看板；做发布决策与回滚预案；安全对抗演练；Eval Agent 能力指标；回顾 | 测试报告、缺陷数据、对抗报告 | 质量看板、发布决策、对抗演练、业务价值确认、回顾 | Gate 4：质量/效率达标、Harness 更新 |

---

## 3. 分阶段实施路径

### 3.1 Spec：让验收"可测"（验收语义契约）
- 用 **BDD / Gherkin**（Given/When/Then）与 **Example Mapping**（黄故事/蓝规则/绿例子/红问题）把模糊意图收敛为 `acceptance_criteria`（来源：Dan North 2006；Gojko Adzic 2011）。
- 每条 AC 带编号（AC-1, AC-2…），作为下游"验什么"的唯一真相源。
- `TASK-*.yaml` 的 `why/what/out` 人类手写（红线①），`acceptance_ref` 回指 AC 编号——**下游 DECOMP 只能引用、禁止自创**（① 人类语义）。
- **测试工程价值**：此阶段产出的 AC 即测试用例的"需求规格"，直接决定 Implement 行为清单与 Gates 的可验证性。

### 3.2 Design：定"怎么验、验多严"（策略与阈值共定）
- 测试分级占比（启发式 **70% 单元 / 20% 集成 / 10% 端到端**；Cohn/Fowler 金字塔，非硬性比例，按系统调整）。
- 门禁阈值由 **Tech Lead + QA 共定**（不可委托 ③），写入 Harness（`scripts/validate-*.sh` 与 `Jenkinsfile`）：
  - 覆盖率 ≥ 80%（line/branch）——**注意覆盖率幻觉**：高覆盖率不等于高有效性（见 §4 变异分）。
  - 变异分 ≥ 60%（④·验证核心证据）。
  - 安全 0 高危。
- 测试数据方案：影子库 + 工厂方法，禁止生产数据入测试；并发/边界用例（如超卖）在 Implement 前预埋。
- **风险分级**：用 `0-References/risk-matrix.md` 对 DECOMP 任务打 `probability×impact`，high 联动 Gate 2 优先审、Implement 优先验（见 §6）。
- 安全设计点（⑤ 判定权在手）：认证/授权、数据保护、红队攻击面标注（STRIDE 威胁建模，Microsoft）。

### 3.3 Implement：TDD 分片实现 + 证据链（Gate 3）
- 每原子任务按 **TDD 红-绿-重构**（Kent Beck）实现：先写失败测试（Red）→ 最小实现通过（Green）→ 在测试保护下重构（Refactor）；单测应亚秒级、确定性、不触网/库。
- **行为清单**（`behavior-checklist.yaml`）定义"验什么"，每条 `maps_to` 一个 AC 编号，禁止自创验收。
- **变异测试（PITest）**作为 ④·验证 核心证据：对改动代码跑变异分析，变异分不达标须补测试；阈值门禁在 PR 卡合并（Atlassian 实践）。
- **Case Delta 审计**：任何测试删/禁用须带 `Test-Disable-Authorization` trailer（⑥），否则 Gate 3 卡住——防止"偷偷删测试过门"。
- **三 A 责任链**（按序不可跳步）：QA 验证（证据可信）→ Reviewer 签字（代码正确）→ PO 收货（业务接受）。自动化门禁只证明格式合规，不能替代人的正确性判断（④ 红线）。

### 3.4 Evaluation：质量度量 + 发布放行 + 对抗（Gate 4）
- **质量看板**：DORA 四指标（部署频率/前置时间/变更失败率/MTTR）+ 变异分趋势 + 缺陷率（<workspace 引 *Accelerate* 2018 / dora.dev>）。
- **发布决策与回滚预案**（⑧ 归 QA）：质量触发条件（如变异分≥60% 且 0 高危 且 缺陷率<1/kloc）+ 回滚验收标准（如 5 分钟内恢复、核心链路可用率≥99.9%）。
- **对抗演练**（⑤ 归安全/红队）：提示注入/越权/越界读 secrets/并发超卖等红卡场景实测，产出对抗报告与合规结论。
- **能力指标**（Eval Agent）：幻觉率/准确率/护栏通过率，落位 `docs/eval/`。
- **回顾**：推动 Harness 规则迭代（⑩），闭环回灌 Design 阈值与 Implement 行为清单。

---

## 4. 度量与门禁阈值体系（带出处）

| 度量 | 建议阈值 | 性质 | 权威出处 |
|------|----------|------|----------|
| 测试分级占比 | 70% 单元 / 20% 集成 / 10% 端到端 | 启发式（按系统调整） | Mike Cohn *Succeeding with Agile*；martinfowler.com/articles/practical-test-pyramid |
| 行/分支覆盖率 | ≥ 80% | 量化阈值（草案） | SDIE `2-Design/test-strategy.template.md`；行业经验值 |
| **变异分（mutation）** | ≥ 60%（PR 门禁） | ④·验证核心证据 | Henry Coles, pitest.org；Atlassian PR 变异门禁实践 |
| 安全扫描 | 0 高危 | 硬门禁 | SDIE Gate 3/4；OWASP / DAST/SAST 实践 |
| Case Delta | 0 未授权删/禁用 | 硬门禁 | SDIE 不可委托 ⑥（commit trailer 授权） |
| DORA 四指标 | 对标 elite 区间 | 效率度量 | Forsgren/Humble/Kim, *Accelerate* (2018), dora.dev |
| 风险等级 | low/med/high = P×I（1–5） | 资源分配依据 | ISTQB CTFL v4.0（Risk=Likelihood×Impact）；`0-References/risk-matrix.md` |

> **覆盖率的幻觉**：行/分支覆盖率只说明"代码被执行"，不说明"测试能发现缺陷"。变异测试是检验测试有效性的黄金标准（pitest.org）——这正是 SDIE 把变异分而非覆盖率作为 ④·验证 核心证据的原因。

---

## 5. 工具链与 Harness 自动化

- **静态/单元层**：linter + 单测框架（亚秒级）+ 变异工具（PITest/Stryker）。
- **集成层**：测试库 + WireMock/本地 fake；窄集成一次测一个外部边界（Fowler）。
- **端到端层**：仅覆盖核心旅程（搜索→加购→结算），不重复低层已覆盖的边界 case。
- **CI 质量门**：`scripts/validate-*.sh` + `Jenkinsfile` 承载 Lint0 / 测试100% / 变异分≥阈值 / 安全0高危 / Case Delta 干净；自动化只证明格式合规，人的三 A 不可省。
- **契约测试**：消费者驱动契约（CDC）保障服务间接口，对齐 DECOMP `acceptance_ref` 与 `context_scope`。
- **Harness 维护（⑩）**由 Dev + Tech Lead 负责，非 Agent 自改。

---

## 6. 风险驱动的测试资源分配

- 公式（ISTQB CTFL v4.0）：**风险等级 = 发生可能性 × 影响程度**（常用 1–5 或 low/med/high）。
- 与工作空间 `0-References/risk-matrix.md` 的 `probability×impact` 量表直接对齐；DECOMP `risk` 字段（high/med/low）即此产出。
- **分配规则**：高风险区 → 更深入全面的覆盖（功能+非功能+接口、边界/负向/压力）；中风险 → 标准覆盖；低风险 → 抽样/探索性测试。
- **持续重评**：缺陷历史、代码复杂度、变更频率（churn）、业务关键度是风险识别输入；每个测试周期重新评估（ISTQB CTAL Test Analyst）。
- 与 SDIE 联动：DECOMP `risk: high` 触发 Gate 2 优先审、Implement 优先验、加强 Review（见 decomposition.template.yml 注释）。

---

## 7. 关键缺口与建议行动（可记入 ADR）

1. **阈值固化**：`2-Design/test-strategy.template.md` 的 80%/60% 为草案，需 TL+QA 共定后写入 Harness 并形成 ADR（不可委托 ③）。
2. **变异分基准**：当前示例为 60–64%，建议按模块风险分级设阶梯阈值（核心模块更高），并以 PR 门禁强制新代码。
3. **测试数据合规**：影子库/工厂方法的落地与脱敏需在 Harness 中固化。
4. **对抗演练常态化**：把红卡场景（并发超卖、提示注入、越权读 secrets）固化为 Evaluation 必跑用例集。
5. **探索性测试补位**：自动化验证已知期望，探索性测试发现未知行为（ISTQB）——AI Coding 下更需人类探索性把关。
6. **测试层级补全**：`2-Design/test-strategy.template.md` 已补 ① 测试层级命名（单元/集成/系统/验收四层）+ 占比、② 验收层级枚举（跨阶段，AC-N→behavior 转自动化测试、Gate 4 签收）、③ 非功能测试类型（可选，按 risk 启用）。
7. **验收结果可追溯**：新增 `4-Evaluation/acceptance-result.template.md`（方案 B，per-AC 判定表）；`coverage_of_ac` 改为由其派生、禁止手填，呼应 ④·验证 红线。

---

## 8. 引用与权威来源

### 8.1 工作空间（SDIE 事实唯一内部权威）
- `SDIE-RACI-Matrix.md`：§1 基础约定、§2 概览矩阵、§2.1 角色说明（④⑤⑥⑦⑧⑨⑩）、§3.x 各阶段 RACI、§5 不可委托清单、§6 Gate 2/3/4。
- `SDIE-Analysis.md`：§3 各阶段详解、§6 不可委托、§7 Gate、§8.1 模板格式选型原则、§9 误区。
- `SDIE-Spec-Guide.md` / `SDIE-Design-Guide.md` / `SDIE-Implement-Guide.md` / `SDIE-Evaluation-Guide.md`：四阶段五问指南。
- `1-Spec/acceptance-criteria.template.md`、`1-Spec/task-spec.template.yaml`、`2-Design/test-strategy.template.md`、`2-Design/decomposition.template.yml`、`3-Implement/behavior-checklist.template.yaml`、`3-Implement/gate3-checklist.template.md`、`4-Evaluation/README.md`、`4-Evaluation/acceptance-result.template.md`。
- `0-References/risk-matrix.md`、`0-References/spec-methods.md`。

### 8.2 外部权威方法论（经权威渠道取得，标注出处）
- Test Pyramid / Practical Test Pyramid：Mike Cohn, *Succeeding with Agile* (2009)；Martin Fowler (Ham Vocke), martinfowler.com/articles/practical-test-pyramid.html
- TDD (Red-Green-Refactor)：Kent Beck, *Test-Driven Development: By Example* (2003)
- BDD / Gherkin：Dan North (2003+)；Cucumber。Specification by Example：Gojko Adzic (2011)
- Mutation Testing / PITest：Henry Coles, pitest.org；Atlassian engineering blog（PR 变异门禁实践）
- Risk-Based Testing：ISTQB CTFL v4.0 / CTAL Test Analyst（Risk = Likelihood × Impact）
- DORA Metrics：Forsgren, Humble, Kim, *Accelerate* (2018), dora.dev
- STRIDE Threat Modeling：Microsoft, learn.microsoft.com/security/threat-modeling
- Conventional Commits：conventionalcommits.org；Trunk-based Development：trunkbaseddevelopment.com
- Google Code Review Practices：google.github.io/eng-practices/review

---

## References

- [The Practical Test Pyramid — Martin Fowler](https://martinfowler.com/articles/practical-test-pyramid.html)
- [PIT Mutation Testing](https://pitest.org)
- [Rovo Dev CLI and Mutation Testing to Write Better Tests — Atlassian](https://www.atlassian.com/blog/how-we-build/rovo-dev-cli-and-mutation-testing-to-write-better-tests)
- [Testing Pyramid Explained for Developers — Wezard](https://wezardapp.com/2026/06/16/software-testing-pyramid-explained-for-developers)
- [Pyramid or Crab? Find a testing strategy that fits — web.dev (Google)](https://web.developers.google.cn/articles/ta-strategies?hl=en)
- [Test-driven development (TDD) — The GDS Way (UK Gov)](https://gds-way.digital.cabinet-office.gov.uk/standards/test-driven-development.html)
- [The Red-Green-Refactor Cycle — HelpMeTest](https://helpmetest.com/blog/tdd-red-green-refactor-cycle)
- [Risk-Based Testing: Prioritizing What Matters Most — Elio Navarrete](https://elionavarrete.com/blog/risk-based-testing)
- [ISTQB CTFL v4.0 Cheat Sheet — ISTQB Guru](https://www.istqb.guru/istqb-ctfl-cheat-sheet/)
- [ISTQB Advanced Level Syllabus Test Analyst (PDF) — GASQ](https://www.gasq.org/files/content/gasq/downloads/certification/ISTQB/Advanced%20Level/Advanced%20Level%20Syllabus%202019-1%20Test%20Analyst.pdf)
- [Mutation Testing With PIT in Java — Java Code Geeks](https://www.javacodegeeks.com/2026/05/06/mutation-testing-with-pit-in-java-the-coverage-metric-youre-ignoring-that-actually-measures-test-quality/)
- [Testing Strategy (Foundations) — ai-solutions.wiki](https://ai-solutions.wiki/foundations/testing-strategy)
