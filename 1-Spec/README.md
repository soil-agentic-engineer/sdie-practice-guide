---
id: DOC-SPEC-DIR-INDEX
title: Spec 阶段产出物文档格式目录索引
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - ../SDIE-Spec-Guide.md
  - ../SDIE-RACI-Matrix.md
last_updated: 2026-08-06
---

# Spec 阶段产出物文档格式（模板目录）

本目录存放 SDIE **Spec 阶段**四类产出物的**文档格式 / 模板**，供实际项目落地时套用。
所有模板均取自工作空间 `SDIE-Spec-Guide.md`（§4 / §6 / §7.1）与 `SDIE-RACI-Matrix.md`（§3.1 / §3.5.1 / §5 / §6）。

## 四类产出物与落位路径（依据 `SDIE-Spec-Guide.md` §4）

| 产出物 | 模板文件 | 推荐落位 | RACI（A） |
|--------|----------|----------|-----------|
| PRD（产品需求文档） | `PRD-template.md` | `docs/specs/` | PM/PO（①） |
| 用户故事地图 User Story Map | `UserStoryMap-template.md` | `docs/specs/story-map/` | PM/PO |
| `acceptance_criteria`（验收标准集） | `acceptance-criteria-template.md` | 内嵌 PRD / TASK-SPEC | PM/PO（①，SME 语义 C、QA 可测 C） |
| `TASK-*.yaml`（结构化任务规格） | `TASK-template.yaml` | `docs/specs/task-specs/*.yaml` | PM/PO（Gate 1） |

## 通用规则（全部来自工作空间，可回溯）

- **frontmatter 七字段**：`id / title / status / phase / owner / related_docs / last_updated`（§7.1）。
- **Spec 阶段 RACI**：PM=A/R，SME=C，Tech Lead=C，Dev=R，QA=C，安全/红队=I，Reviewer=I（§3.1 / §3.5.1）。
- **Gate 1 准入标准**：Task Spec 完整、AGENTS.md 最新、验收可测；A=PM，C=QA 把关验收可测（§6）。
- **不可委托 ①**：业务需求与验收语义拍板 → PM/SME，Agent 仅补草稿（§5）。
- **status 状态机**：`draft → review → baseline → change → superseded`；只有 `baseline` 才能过 Gate 1（§7.2）。
- **版本化**：SemVer 2.0.0 + ADR 决策记录 + Git 约定（§7.3 / §7.4 / §7.6）。

> 权威性原则：本目录模板的 SDIE 事实均取自工作空间 `SDIE-RACI-Matrix.md` 与 `SDIE-Spec-Guide.md`；
> 方法论（Impact Mapping / User Story Mapping / INVEST / BDD 等）标注为权威推荐，来源见 `SDIE-Spec-Guide.md` §8.2。
