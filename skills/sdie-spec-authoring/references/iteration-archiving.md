---
id: DOC-SPEC-ITER-001
title: 跨阶段迭代归档（docs/iterations/iter-NNN/）
status: draft
phase: Spec
owner: PM / 产品负责人
related_docs:
  - spec-dir-structure.md
  - spec-guide.md
  - prd.template.md
  - user-story-map.template.md
  - acceptance-criteria.template.md
  - task-spec.template.yaml
last_updated: 2026-08-28
---

# 跨阶段迭代归档（docs/iterations/iter-NNN/）

> 本文件从 **Spec 阶段写作视角** 描述"跨阶段迭代归档"：即业务域仓 `<domain>/docs/iterations/iter-NNN/` 的构成、Spec 产出物如何进入归档、以及 Spec 作者在归档中的职责边界。
>
> 归档本身是 **跨阶段制品**（覆盖 Spec → Design → Implement → Evaluation），不归任一阶段私有；本文档只讲 Spec 侧**怎么用、贡献什么、边界在哪**，避免越界代写其他阶段的归档内容。

---

## 0. 为什么需要跨阶段迭代归档

- **一轮迭代（iter-NNN）= 一个可发布增量**：把本轮从需求（Spec）到上线（Eval）的所有制品、门禁结论、发布说明、复盘**绑在一个目录里**，形成闭环、可回溯。
- **跨阶段串联**：单份 PRD / AC 只描述"要什么"，迭代归档描述"这一轮我们**实际交付了什么、过没过门、为何能发、下次改什么**"。
- **域仓本地闭环**：`iter-NNN/` 落在 `<domain>/docs/iterations/`，与 `1-Spec/…4-Evaluation/` 同仓，不依赖 `meta-sdie` 被 checkout 即可追溯本轮全貌。
- **系统级兜底**：多个域仓的 `iter-NNN` 在 `meta-sdie/releases/train-vX/` 被聚合编排（见 §4）。

---

## 1. 归档结构（iter-NNN）

```
<domain>/docs/iterations/
└── iter-NNN/                      # 一轮迭代（NNN 三位序号，如 iter-001）
    ├── ITER-PLAN.md               # 本轮目标 / 范围 / 跨阶段计划（Spec 起笔目标，全阶段共建）
    ├── MANIFEST.md                # 本轮所有制品 → 版本映射（含 Spec 四件产出物）
    ├── gates/                     # 四道门禁核查（gate-1.md … gate-4.md）
    │   ├── gate-1.md              # ← Spec 阶段出口门禁（PM 签 / QA 复核验收可测）
    │   ├── gate-2.md              #   Design 出口（非 Spec 侧）
    │   ├── gate-3.md              #   Implement 出口（非 Spec 侧）
    │   └── gate-4.md              #   Evaluation 出口（非 Spec 侧）
    ├── RELEASE-NOTES.md           # 本轮发布说明（全阶段共建，非 Spec 独有）
    └── RETRO.md                   # 无责复盘（phase: Evaluation，非 Spec 侧）
```

> 命名说明：根约定用短名 `ITER-PLAN / MANIFEST / RELEASE-NOTES / RETRO`；本仓库统一加 `.md` 以契合 frontmatter / 模板体系。`RETRO` 的元信息模板见 `4-Evaluate/retrospective.template.md`（`id: RETRO-<ITER>`，`phase: Evaluation`）。

---

## 2. Spec 产出物如何进入归档

| 时机 | 动作 | 落点 |
|------|------|------|
| Spec 产出物写完草稿 | `status: draft`，红线字段标"待人类确认" | `docs/1-Spec/<子目录>/<id>` |
| **Gate 1 通过**（PM 签、QA 复核验收可测） | 人类将 `status` 升 `baseline` | 同左（**Agent 不得自行置 baseline**） |
| 升 baseline 后、纳入本轮迭代 | Spec 四件（`prd/`、`user-story-map/`、`acceptance_criteria/`、`task-specs/`）的 `id`+版本登记进 `MANIFEST.md` | `iter-NNN/MANIFEST.md` |
| 迭代启动 / 目标对齐 | PRD 的 Goal、用户故事地图的范围作为 `ITER-PLAN.md` 的目标与范围输入 | `iter-NNN/ITER-PLAN.md` |
| 其他阶段推进 | Design/Implement/Evaluation 各自补 `gate-2…4`、`RELEASE-NOTES`、`RETRO` | 对应文件（**非 Spec 侧职责**） |

> **关键约束**：本 Skill **只做落位认知与提示**，不主动创建 `iter-NNN/` 目录树，也不代写 `gate-1.md` 的签核结论。Skill 在交付 Spec 文档时，仅提示"该文档经 Gate 1 升 baseline 后将归档至 `docs/iterations/iter-NNN/`"。

---

## 3. Spec 在归档中的职责边界

**Spec 侧负责（可贡献）：**
- `ITER-PLAN.md` 的**目标与范围**部分（源自 PRD Goal / 用户故事地图主题）。
- `MANIFEST.md` 中 **Spec 四件产出物** 的条目（`id`、版本、`phase: Spec`、`status: baseline`）。
- `gate-1.md` 的 **Spec 阶段出口核查**（七字段齐全、需求可追溯、AC 可测且 QA 已复核、优先级双维已定、不可委托项已转人类、关联文档互链——即 Gate 1 六项清单）。

**Spec 侧不负责（红线，不代写）：**
- `gate-2 / gate-3 / gate-4`：属 Design / Implement / Evaluation 出口门禁。
- `RELEASE-NOTES.md`：跨阶段发布说明，由全阶段共建。
- `RETRO.md`：无责复盘，牵头为 Reviewer（C），`phase: Evaluation`。
- 任何把 `status` 直接置 `baseline` 的动作（红线：须 PM 签、Gate 1 通过）。

---

## 4. 与 meta-sdie 发布列车（train-vX）的关系

```
meta-sdie/releases/
└── train-vX.Y.Z/                  # 系统级跨域发布列车（聚合多域仓多轮迭代）
    ├── MANIFEST.md                # 各 <domain> 仓 → 版本映射（含其 iter-NNN）
    ├── gate-1.md … gate-4.md      # 系统级四道门禁核查
    └── RELEASE-NOTES.md
```

- `iter-NNN/` 是 **域仓本地**、**单轮**迭代闭环；`train-vX/` 是 **治理仓全局**、**跨域跨轮**发布编排。
- Spec 产出物先在本域 `iter-NNN/MANIFEST.md` 登记，再随域仓版本被 `train-vX/MANIFEST.md` 聚合。
- Spec 作者只需关心本域 `iter-NNN`；系统级编排由 Tech Lead / 治理层在 `meta-sdie` 完成。

---

## 5. 与 Skill 的配合

- 本 Skill 在生成 PRD / 用户故事地图 / 验收标准 / TASK 时，仅**提示落位与归档路径**，不创建 `iter-NNN/` 目录、不代签 Gate 1。
- 收尾提示固定话术："本 Spec 文档落位 `<domain>/docs/1-Spec/...`；经 PM 签 Gate 1、QA 复核验收可测并升 `baseline` 后，将登记进当轮 `docs/iterations/iter-NNN/MANIFEST.md`，并作为 `ITER-PLAN` 的目标/范围输入。"
- 跨阶段归档的完整结构见本文件；Spec 阶段目录本身见 `@references/spec-dir-structure.md`，二者互补、不重复。
