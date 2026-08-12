# Evaluation 阶段产出物格式模板索引

> 本目录是 SDIE **Evaluation（评估）阶段**各产出物的**文档格式模板**，配合 `SDIE-Evaluation-Guide.md` 使用。
> 所有模板均带 **元信息七字段**（Markdown 第 1 节 `## 元信息（meta）` / YAML `meta:` 块下挂；id / title / status / phase / owner / related_docs / last_updated），
> 并标注对应 RACI / Gate 4 / 不可委托红线。填用时删除示例注释即可。

## 通用规则（来自 SDIE-RACI-Matrix.md）
- **阶段 A**：QA / 测试架构师（发布放行不可委托 ⑧）。**PM 在本阶段降为 C**（仅签业务价值确认，非技术放行 A）。
- **Gate 4 准入**：质量/效率指标达标、回顾已开、Harness 规则已更新。
- **不可委托**：⑧ 发布决策与回滚预案（QA）、⑤ 安全判定与合规结论（安全/红队）、⑩ Harness 维护（Dev+TL）。
- **status 状态机**：`draft → review → baseline → change → superseded`；只有 `baseline` 才能过 Gate 4。
- **版本化**：SemVer 2.0.0 + ADR + Git 发布闭环（详见指南 §7）。
- **版本历史段落（方案 A）**：每篇产出物正文末尾须维护 `## 版本历史`（§6.5，不进元信息七字段）；权威溯源见 `0-References/changelog.md`。

## 模板清单

| 模板 | 产出物 | 主要作者（R） | A（问责） |
|------|--------|--------------|-----------|
| `Quality-Dashboard-template.md` | 质量看板 / 度量报告 | QA（R） | QA（⑧） |
| `Release-Decision-template.md` | 发布决策 + 回滚预案 | QA（R/A） | QA（⑧） |
| `Adversarial-Report-template.md` | 对抗演练报告 | 安全/红队（C） | 安全/红队（⑤） |
| `Business-Value-Confirmation-template.md` | 业务价值确认 | PM/PO（C） | PM/PO（签确认，非 A） |
| `Eval-Metrics-template.yaml` | 能力指标报告（Eval Agent） | Eval Agent（●） | QA/TL 复核 |
| `Retrospective-template.md` | 回顾 / Retrospective | Reviewer（C）牵头 | 团队（知会） |

## 落位建议
- 统一落位 `docs/eval/`（与 `SDIE-RACI-Matrix.md` §4 Eval Agent 落位一致）。
- 关联文档通过元信息 `related_docs` 互链，形成可追溯交付包。
