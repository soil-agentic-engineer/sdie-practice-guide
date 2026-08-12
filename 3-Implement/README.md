# Implement 阶段产出物格式模板索引

> 本目录是 SDIE **Implement（实现）阶段**各产出物的**文档格式模板**，配合 `SDIE-Implement-Guide.md` 使用。
> 所有模板均带 **元信息七字段**（Markdown 第 1 节 `## 元信息（meta）` / YAML `meta:` 块下挂；id / title / status / phase / owner / related_docs / last_updated），
> 并标注对应 RACI / Gate 3 / 不可委托红线。填用时删除示例注释即可。

## 通用规则（来自 SDIE-RACI-Matrix.md）
- **阶段三 A**：QA（④·验证）→ Reviewer（④·签字）→ PO（⑦ 收货合并），按发生顺序串联。
- **Gate 3 准入**：Lint 0 违规、测试 100% 通过、安全 0 高危、Case Delta 无未授权删/禁用、PR 标签正确、Review 通过。
- **不可委托**：④ 正确性问责（验证+签字）、⑤ 安全判定、⑥ 测试删/禁用授权、⑦ 收货合并、⑨ 越级拦截。
- **status 状态机**：`draft → review → baseline → change → superseded`；只有 `baseline` 才能过 Gate 3。
- **版本化**：SemVer 2.0.0 + ADR + Git 三 A 串联变更闭环（详见指南 §7）。
- **版本历史段落（方案 A）**：每篇产出物正文末尾须维护 `## 版本历史`（§6.5，不进元信息七字段）；权威溯源见 `0-References/changelog.md`。

## 模板清单

| 模板 | 产出物 | 主要作者（R） | A（问责） |
|------|--------|--------------|-----------|
| `PR-template.md` | PR + 自验 + ⑥ commit trailer | Dev (Task Owner) | PO（⑦）/ QA（④·验证） |
| `Behavior-Checklist-template.yaml` | 行为清单 | Test Designer（◐） | QA / Reviewer |
| `Case-Delta-template.md` | 测试删/禁用审计报告（⑥） | Dev / 安全 | 提交 trailer 的人类（⑥） |
| `Review-Checklist-template.md` | Review 清单（④·签字 依据） | Review Agent（建议） | Reviewer（④·签字） |
| `Gate3-Checklist-template.md` | Gate 3 准入自检 | Dev (Task Owner) | QA→Reviewer→PO |

## 落位建议
- 实现代码/测试：`src/`、`tests/`；PR 评论为 Review 建议。
- 行为清单：随 `docs/specs/task-specs/*.yaml`（与 Spec Agent 落位一致）。
- Case Delta / Gate3 自检：随 PR 提交或 `docs/implement/`。
