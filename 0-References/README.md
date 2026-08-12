# 0-References — 权威方法引用库

> 本目录专用于存储 SDIE 知识库在构建与演进过程中**引用到的外部权威方法 / 框架**的知识与溯源。
> 遵循本项目的「权威性原则」：SDIE 核心事实仅取自工作空间（`SDIE-RACI-Matrix.md` 等）；
> 当知识不在工作空间内时，须经由权威渠道（官方文档 / 标准组织 / 原作者论著）取得并标注出处。

## 本目录的定位
- **不是 SDIE 的阶段产出物**：本目录文档**不挂元信息七字段**，而用「引用来源」元数据块，以明确区分"外部权威知识"与"空间内 SDIE 约定"。
- **是引用溯源中枢**：每份方法文档记录其官方定义、核心要素、在 SDIE 中的用法、以及权威出处，便于回溯与复核。
- **随引用增长而扩充**：每当 SDIE 指南 / 模板引用新的外部方法，应在此补一份同格式文档，并在下方索引登记。

## 权威性原则（摘要）
- 工作空间内 SDIE 事实 = 唯一权威基准（`SDIE-RACI-Matrix.md` 7/12 治理版 + `SDIE-Analysis.md`）。
- 外部方法论 = 经 WebSearch / WebFetch 取得的权威定义，必须标注出处（作者 / 机构 / 年份 / URL）。
- **严禁引用工作空间以外的本地其他目录**（被判定为非权威）。

## 方法索引

| 方法 | 类别 | 权威来源 | 在 SDIE 中的引用位置 |
|------|------|----------|----------------------|
| SemVer 2.0.0 | 版本化 | semver.org（T. Preston-Werner, 2013） | 各阶段指南 §7、README「通用约定」 |
| ADR（架构决策记录） | 决策记录 | M. Nygard, 2011 | SDIE-Spec-Guide §6.4、2-Design/ADR-template.md |
| KANO 模型 | 需求优先级 | N. Kano et al., 1984 | PRD-template §7、SDIE-Spec-Guide §5.2 |
| MoSCoW | 需求优先级 | D. Clegg, 1994 / DSDM | PRD-template §7、SDIE-Spec-Guide §5.2 |
| WBS / PMBOK | 任务分解 | PMI（PMBOK Guide） | SDIE-Design-Guide §6.2、2-Design/Decomposition-template.yml |
| Story Points / Planning Poker | 估算 | M. Cohn / R. Jeffries / J. Grenning | Decomposition 讨论（task `estimate` 字段） |
| Risk Matrix（概率×影响） | 风险管理 | PMBOK 7th / ISO 31000:2018 | Decomposition 讨论（task `risk` 字段） |
| Agent Task Complexity（L1–L5 可执行性分级） | 任务复杂度分级 | Temporal L1–L5 / SWE-bench 结构因子 / Parasuraman LOA | Decomposition 讨论（task `estimated_complexity` 字段） |
| Spec 阶段方法集（Impact Mapping / User Story Mapping / INVEST / BDD-Gherkin / Spec-by-Example / Event Storming） | 需求 / 规格方法论 | Adzic / Patton / Wake / North / Brandolini | SDIE-Spec-Guide §5 |
| Docs-as-Code（YAML 元数据块） | 文档工程 | Jekyll / Hugo / MkDocs 生态 | SDIE-Spec-Guide §6.1（元信息载体） |
| Git / Conventional Commits / trunk-based | 版本控制约定 | Git / conventionalcommits.org / P. Hammant | 各阶段指南 §7、README「通用约定」 |
| CHANGELOG / 版本变化说明（Keep a Changelog） | 版本化配套 | O. Lacan, 2016 (keepachangelog.com) / IEEE 828-2012 变更记录 | SDIE-Spec-Guide §6.5（方案 A）、0-References/changelog.md |

## 文档模板（新增方法时套用）
每份方法文档采用六段式：
1. **引用来源**：权威出处（作者 / 机构 / 年份 / URL）
2. **在 SDIE 中引用位置**：哪个文件 §章节引用了它
3. **用途**：一句话说明为何引用
4. **定义**：官方 / 标准定义
5. **核心要素**：关键组成
6. **在 SDIE 中的用法**：如何被 SDIE 使用、与哪些字段 / 红线联动
7. **权威出处**：可核验的来源清单
