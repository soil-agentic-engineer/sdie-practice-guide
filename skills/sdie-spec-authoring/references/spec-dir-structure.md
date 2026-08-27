---
id: DOC-SPEC-DIR-001
title: Spec 阶段目录结构（docs/1-Spec/ 落位）
status: draft
phase: Spec
owner: PM / 产品负责人
related_docs:
  - spec-guide.md
  - prd.template.md
  - user-story-map.template.md
  - acceptance-criteria.template.md
  - task-spec.template.yaml
last_updated: 2026-08-27
---

# Spec 阶段目录结构（docs/1-Spec/）

> 本文件**只覆盖 SDIE Spec 阶段**的目录结构——即业务域仓中 `docs/1-Spec/` 之下，四类 Spec 产出物（PRD / 用户故事地图 / 验收标准 / TASK 规格）及其模板副本的落位约定。
> 占位符：`<domain>`（业务域仓）、`<id>`（产出物唯一标识，建议语义化短名或序号）。

---

## 0. 父级定位（仅作方位参照）

```
<domain>/docs/                 # 本域 SDIE AI Coding 制品（四阶段，各阶段独立目录）
├── 1-Spec/                    # ★ 本文件覆盖范围
├── 2-Design/                  # Design 阶段（不在本文件范围）
├── 3-Implement/               # Implement 阶段（不在本文件范围）
├── 4-Evaluation/              # Evaluation 阶段（不在本文件范围）
└── iterations/                # 跨阶段迭代归档（结构见 iteration-archiving.md，不在本文件范围）
```

> 本文件只展开 `1-Spec/`；其余阶段目录结构见对应阶段文档。

---

## 1. Spec 阶段目录结构（docs/1-Spec/）

```
<domain>/docs/1-Spec/
├── _templates/                # ★ Spec 四模板「受控副本」（由 meta-sdie 推送钉选版本，局部可用）
│   ├── prd.template.md
│   ├── user-story-map.template.md
│   ├── acceptance-criteria.template.md
│   └── task-spec.template.yaml
├── prd/                       # 产品需求文档（一份需求域一个文件）
│   └── <id>.md
├── user-story-map/            # 用户故事地图（按版本 / 主题一个文件）
│   └── <id>.md
├── acceptance_criteria/       # 验收标准集（BDD/Gherkin；可执行映射见 e2e/features/）
│   └── <id>.md
└── task-specs/                # TASK 结构化任务规格（yaml）
    └── TASK-<id>.yaml
```

> **落位原则**：四类产出物各归其目录；先写产出物草稿，再由人类（PM）审定、Gate 1 通过后升 `baseline`。
> `e2e/features/` 的 Gherkin 文件是验收标准的**可执行映射**，由 Acceptance Criteria 派生，不在此目录内生成（属 Implement/Evaluation 落位）。

---

## 2. 产出物 → 落位路径映射

| 产出物 | 模板（`_templates/`） | 落位路径 | `frontmatter.phase` |
|--------|----------------------|----------|---------------------|
| PRD | `prd.template.md` | `<domain>/docs/1-Spec/prd/<id>.md` | Spec |
| 用户故事地图 | `user-story-map.template.md` | `<domain>/docs/1-Spec/user-story-map/<id>.md` | Spec |
| 验收标准 | `acceptance-criteria.template.md` | `<domain>/docs/1-Spec/acceptance_criteria/<id>.md` | Spec |
| TASK 规格 | `task-spec.template.yaml` | `<domain>/docs/1-Spec/task-specs/TASK-<id>.yaml` | Spec |

> 所有 Spec 产出物统一带 **frontmatter 七字段**、`phase: Spec`、`status` 状态机（`draft → review → baseline → change → superseded`）。
> `status` 不得由 Agent 直接置 `baseline`（需 Gate 1 通过：PM 签、QA 复核验收可测）。

---

## 3. 模板治理（Spec 侧口径）

- `_templates/` 是 meta-sdie 推送的**受控副本**：本地可用、构建密闭（域仓 CI 不依赖 meta-sdie 被 checkout）。
- 域仓 Agent 生成本域 PRD / 故事地图 / 验收标准 / TASK 时，在本仓 `1-Spec/_templates/` 内取用规范模板，与 `docs/1-Spec/` 同仓闭环、可追溯。
- 模板结构 / 字段变更属治理变更，须由 **Tech Lead 审定**后统一下发；域仓不得私自改写副本内容。

---

## 4. 与 Skill 的配合

- 本 Skill 的 `@references/*.template.*` 为生成时所用规范模板；生成结果按 §2 落位到 `<domain>/docs/1-Spec/...`。
- Skill 仅做**落位认知与提示**，不主动创建目录树；仅在用户明确要求初始化脚手架、且 Tech Lead 审定结构后，才据本文件生成骨架。
