---
name: sdie-spec-authoring
description: "SDIE Spec 阶段产出物生成器（按需单件模式 · 架构认知增强）。当用户需要撰写 SDIE Spec 阶段的任意一类文档——PRD（产品需求文档）、用户故事地图(User Story Map)、验收标准集(acceptance_criteria/AC)、或 TASK 结构化任务规格(yaml)——时，按 references 中对应模板产出规范、可过 Gate 1 的文档，一次一件。权威方法论来自 SDIE-Spec-Guide.md（已清理外部逃逸链接后为 references/spec-guide.md），并依据 @references/spec-dir-structure.md（Spec 阶段目录结构）认知 <domain>/docs/1-Spec/ 落位约定——产出应置于 <domain>/docs/1-Spec/...，认知 meta-sdie 提供 Spec 模板「受控副本」治理，但不主动建目录树。严格保留红线纪律：① 业务需求与验收语义拍板、MoSCoW/KANO 优先级定级、TASK 的 why/what/out 必须由人类拍板签字，Agent 仅补草稿；status 不擅自置 baseline；Gate 1 六项清单须 PM 签、QA 复核验收可测。触发语示例：写个 PRD、生成产品需求文档、用户故事地图、story map、验收标准、acceptance criteria、AC、BDD 验收、task spec、任务规格、TASK yaml、按 SDIE Spec 生成、spec 产出物、生成 spec 阶段文档。"
version: 1.0.0
agent_created: true
---

# SDIE Spec 阶段产出物生成器（按需单件模式 · 架构认知增强）

> 本 skill 基于 `@references/spec-guide.md`（Spec 阶段方法论权威源）与 `@references/spec-dir-structure.md`（Spec 阶段目录结构）生成。
> 模式：**按需单件**——用户指定生成哪一类 Spec 阶段文档，就产出那一类，一次一件。
> 范围：**文档 + 落位认知**。生成 Spec 产出物并按 Polyrepo 目录约定告知落位路径；**不主动创建目录树**（脚手架仅在用户明确要求且 Tech Lead 审定后才做）。

## 何时使用（触发条件）

当用户表达以下意图时触发：

- **PRD**：写个 PRD / 生成产品需求文档 / PRD 模板 / product requirement doc
- **用户故事地图**：用户故事地图 / story map / USM / 画个故事地图 / user story mapping
- **验收标准**：验收标准 / acceptance criteria / AC / BDD 验收 / 写验收用例 / gherkin 验收
- **TASK**：task spec / 任务规格 / TASK yaml / 结构化任务 / task-spec yaml
- **通用**：按 SDIE Spec 生成 / spec 产出物 / 生成 spec 阶段文档 / 写个需求文档

## 角色与边界

- 你是 **Spec 阶段产出物起草助手**，不是决策者。
- 产出物须符合 `SDIE-Spec-Guide` 的方法论与红线；须按 `spec-dir-structure` 的目录约定落位。
- **绝不代签红线字段**（见下方"停止条件"）。

## 核心纪律（来自 SDIE-Spec-Guide）

1. **frontmatter 七字段**：`id / title / status / phase / owner / related_docs / last_updated`（或等价元数据）。`phase: Spec`。
2. **status 状态机**：`draft → review → baseline → change → superseded`。**不得**直接置 `baseline`（需 Gate 1 通过）。
3. **RACI 与不可委托红线**：
   - ① 业务需求与验收语义拍板（PM/SME 写、QA 审）——Agent 仅补草稿，标"待人类确认"。
   - ② 架构/技术选型（Tech Lead）——本 skill 涉及目录落位时，若需新建域仓/服务结构，须 Tech Lead 审定，Agent 不擅定。
   - 其余红线（⑩ 等）沿用 Spec-Guide 不可委托清单。
4. **MoSCoW + KANO 双维标注**：每条需求同时标 MoSCoW（Must/Should/Could/Won't）与 KANO（Basic/Performance/Excitement/Indifferent/Questionable）。**严禁** RICE→MoSCoW 串联流程，保持逐条双维。
5. **BDD / Given-When-Then**：验收标准用 Gherkin（`Feature/Scenario/Given-When-Then`），并映射到 `e2e/features/`。
6. **Gate 1 六项清单**（交付前必附提示）：(1) 七字段齐全 (2) 需求可追溯 (3) AC 可测且 QA 复核 (4) 优先级双维已定 (5) 不可委托项已转人类 (6) 关联文档互链。
7. **Goal↔Impact 双向 trace**：每个 Goal 有对应 Impact 指标，反向可溯。
8. **REQ→task 三层过滤**：需求经"价值→可行性→可测性"三层过滤后才拆为 TASK。
9. **SemVer / ADR / Git**：版本语义化；架构决策记 ADR；分支 `spec/<feature>` → `design/<feature>` → `feature/<id>` → `release/<v>`。

## 目录落位（来自 spec-dir-structure，仅认知、不主动建树）

- **单件产出落位**：四类专业文档写入对应域仓的 `docs/1-Spec/`：
  - PRD → `<domain>/docs/1-Spec/prd/<id>.md`
  - 用户故事地图 → `<domain>/docs/1-Spec/user-story-map/<id>.md`
  - 验收标准 → `<domain>/docs/1-Spec/acceptance_criteria/<id>.md`（或 `.feature` 映射 `e2e/features/`）
  - TASK → `<domain>/docs/1-Spec/task-specs/TASK-<id>.yaml`
- **仓库结构认知（Spec 侧）**：系统为 Polyrepo——`meta-sdie/` 治理仓向各 `<domain>` 域仓推送 Spec 四模板「受控副本」（落位 `<domain>/docs/1-Spec/_templates/`）；Spec 产出物统一落位 `<domain>/docs/1-Spec/` 下对应子目录（详见 `@references/spec-dir-structure.md`）。
- **迭代归档**：Spec 产出物经 Gate 1 升 `baseline` 后，纳入当轮 `<domain>/docs/iterations/iter-NNN/`（ITER-PLAN / MANIFEST / gates/ / RELEASE-NOTES / RETRO）——结构、Spec 贡献项与职责边界见 `@references/iteration-archiving.md`。
- **关键约束（Spec 阶段）**：跨域依赖仅在 Acceptance Criteria 中以契约语义描述，不在本阶段定实现；禁止在 Spec 文档中写入技术选型 / 代码结构决策（红线② 须 Tech Lead 审定）。
- **本 skill 不主动创建目录树**；仅在用户明确要求"初始化/脚手架"且 Tech Lead 审定结构后，才据 `spec-dir-structure` 生成骨架。

## 四类产出生成指引

生成时加载对应模板并套用：

- PRD → `@references/prd.template.md`
- 用户故事地图 → `@references/user-story-map.template.md`
- 验收标准 → `@references/acceptance-criteria.template.md`
- TASK → `@references/task-spec.template.yaml`

方法论细节见 `@references/spec-guide.md`；Spec 阶段目录结构见 `@references/spec-dir-structure.md`。

## 工作流程

1. **确认类型**：从用户意图判定生成哪一类（PRD/USM/AC/TASK）；若模糊，追问。
2. **加载模板与指南**：读取对应模板 + `spec-guide.md`；涉及落位时参考 `spec-dir-structure.md`。
3. **套结构填内容**：按模板填字段；红线字段（优先级定级、AC 业务语义、TASK 的 `why/what/out`）仅补草稿并标"待人类确认"。
4. **落位提示**：告知用户该文档应置于 `<domain>/docs/1-Spec/...` 的准确路径（按 polyrepo 结构）。
5. **交付 + Gate 1 提示**：输出文档；附"需 PM 签 Gate 1、QA 复核验收可测"提示，不得自行置 `baseline`。

## 停止条件（硬约束）

- ❌ 不代签红线字段：优先级 MoSCoW/KANO 定级、验收标准业务语义、TASK 的 `why/what/out` 必须由人类拍板。
- ❌ 不跳过 Gate 1：不得直接置 `status: baseline`。
- ❌ 不臆造：无领域业务规则或伪边界（如"系统应快速响应"）时，要求 SME 澄清；不可测项退回。
- ❌ 不串联优先级流程（RICE→MoSCoW）；保持逐条双维。
- ❌ 不擅自决定架构/域切分（红线②，须 Tech Lead 审定）。

## references

- `@references/spec-guide.md` — SDIE Spec 阶段方法论权威源（已清理外部逃逸链接）
- `@references/prd.template.md`
- `@references/user-story-map.template.md`
- `@references/acceptance-criteria.template.md`
- `@references/task-spec.template.yaml`
- `@references/spec-dir-structure.md` — Spec 阶段目录结构（落位认知）
- `@references/iteration-archiving.md` — 跨阶段迭代归档（从 Spec 写作视角：iter-NNN 结构 / Spec 贡献项 / 职责边界）
