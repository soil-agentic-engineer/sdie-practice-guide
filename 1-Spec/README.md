## 元信息（meta）

```yaml
id: DOC-SPEC-DIR-INDEX
title: Spec 阶段产出物文档格式目录索引
status: draft
phase: Spec
owner: PM/PO
related_docs:
  - ../SDIE-Spec-Guide.md
  - ../SDIE-RACI-Matrix.md
last_updated: 2026-08-06
```

# Spec 阶段产出物文档格式（模板目录）

本目录存放 SDIE **Spec 阶段**四类产出物的**文档格式 / 模板**，供实际项目落地时套用。
所有模板均取自工作空间 `SDIE-Spec-Guide.md`（§4 / §6.1）与 `SDIE-RACI-Matrix.md`（§3.1 / §3.5.1 / §5 / §6）。

## 四类产出物与落位路径（依据 `SDIE-Spec-Guide.md` §4）

| 产出物 | 模板文件 | 推荐落位 | RACI（A） |
|--------|----------|----------|-----------|
| PRD（产品需求文档） | `prd.template.md` | `docs/specs/` | PM/PO（①） |
| 用户故事地图 User Story Map | `user-story-map.template.md` | `docs/specs/story-map/` | PM/PO |
| `acceptance_criteria`（验收标准集） | `acceptance-criteria.template.md` | 内嵌 PRD / TASK-SPEC | PM/PO（①，SME 语义 C、QA 可测 C） |
| `TASK-*.yaml`（结构化任务规格） | `task-spec.template.yaml` | `docs/specs/task-specs/*.yaml` | PM/PO（Gate 1） |

## 通用规则（全部来自工作空间，可回溯）

- **元信息七字段**（第 1 节 `## 元信息（meta）`，YAML 代码块）：`id / title / status / phase / owner / related_docs / last_updated`（§6.1）。
- **Spec 阶段 RACI**：PM=A/R，SME=C，Tech Lead=C，Dev=R，QA=C，安全/红队=I，Reviewer=I（§3.1 / §3.5.1）。
- **Gate 1 准入标准**：Task Spec 完整、AGENTS.md 最新、验收可测；A=PM，C=QA 把关验收可测（§6）。
- **不可委托 ①**：业务需求与验收语义拍板 → PM/SME，Agent 仅补草稿（§5）。
- **status 状态机**：`draft → review → baseline → change → superseded`；只有 `baseline` 才能过 Gate 1（§6.2）。
- **版本化**：SemVer 2.0.0 + ADR 决策记录 + Git 约定（§6.3 / §6.4 / §6.6）。
- **版本历史段落（方案 A）**：每篇基线化产出物正文末尾须维护 `## 版本历史`（版本/日期/变更摘要/关联 ADR），不进元信息七字段（§6.5）；权威溯源见 `0-References/changelog.md`。

> 权威性原则：本目录模板的 SDIE 事实均取自工作空间 `SDIE-RACI-Matrix.md` 与 `SDIE-Spec-Guide.md`；
> 方法论（Impact Mapping / User Story Mapping / INVEST / BDD 等）标注为权威推荐，来源见 `SDIE-Spec-Guide.md` §8.2。

## 多 Task 的组织约定

一个 Feature 的多个通过三层过滤（§4.5）的 REQ，各自生成独立的
`TASK-<FEATURE>-NNN.yaml`（一文件一任务，便于独立 baseline / 过 Gate 1 / SemVer）。
建议按 feature 目录聚合，并配一份 Markdown 索引导航：

```
docs/specs/task-specs/
└── <feature>/
    ├── TASK-<FEATURE>-001.yaml
    ├── TASK-<FEATURE>-002.yaml
    └── README.md   ← 本 feature 下 Task Spec 索引（状态 / AC 覆盖，非 YAML、不进 meta 七字段）
```

单 REQ 内的 1:N 细粒度拆解留给 Design 阶段的 `DECOMP-*`（§4.5）。
