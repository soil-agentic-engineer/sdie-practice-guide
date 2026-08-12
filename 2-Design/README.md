# Design 阶段产出物格式模板索引

> 本目录是 SDIE **Design（设计）阶段**各产出物的**文档格式模板**，配合 `SDIE-Design-Guide.md` 使用。
> 所有模板均带 **元信息七字段**（Markdown 第 1 节 `## 元信息（meta）` / YAML `meta:` 块下挂；id / title / status / phase / owner / related_docs / last_updated），
> 并标注对应 RACI / Gate 2 / 不可委托红线。填用时删除示例注释即可。

## 通用规则（来自 SDIE-RACI-Matrix.md）
- **阶段 A**：Tech Lead / 架构负责人（Gate 2 审批）。
- **Gate 2 准入**：分解合理、上下文策略就绪、Harness 确认。
- **不可委托**：② 架构选型与 ADR 定稿（Tech Lead）、③ 门禁阈值共定（Tech Lead+QA）、⑩ Harness（Dev+TL）。
- **status 状态机**：`draft → review → baseline → change → superseded`；只有 `baseline` 才能过 Gate 2。
- **版本化**：SemVer 2.0.0 + ADR 决策记录 + Git 基线化变更闭环（详见指南 §7）。
- **版本历史段落（方案 A）**：每篇产出物正文末尾须维护 `## 版本历史`（§6.5，不进元信息七字段）；权威溯源见 `0-References/changelog.md`。

## 模板清单

| 模板 | 产出物 | 主要作者（R） | A（问责） |
|------|--------|--------------|-----------|
| `ADR-template.md` | ADR 架构决策记录 | Tech Lead（Design Agent 起草） | Tech Lead（②） |
| `Decomposition-template.yml` | 原子分解方案 | Dev (Task Owner) | Tech Lead（Gate 2） |
| `Test-Strategy-template.md` | 测试策略 + 门禁阈值草案 | QA（C，草案） | Tech Lead+QA（③ 共定） |
| `Context-Injection-template.md` | 上下文注入策略 | Tech Lead（R/A） | Tech Lead |
| `Security-Design-template.md` | 安全设计点 | 安全/红队（C） | 安全/红队（⑤ 判定权） |

## 落位建议
- ADR / 分解 / 测试策略 / 上下文注入 / 安全设计：统一落位 `docs/design/`（与 `SDIE-RACI-Matrix.md` §4 Design Agent 落位一致）。
- 关联文档通过元信息 `related_docs` 互链，形成可追溯 Design 包。

## Decomposition-template.yml 字段提示
- task 级字段：**六必填** `id` / `title` / `agent_assignable` / `depends_on` / `acceptance_ref` / `context_scope` + **两可选** `risk`、`estimated_complexity`。
- **可选 `risk` 字段**：`{ probability, impact, level }`，仅对带不确定性/后果严重的 task 标注；`probability×impact` 1–5 量表与三档阈值（low 1–6 / med 7–12 / high 13–25，阈值由团队定）见 `0-References/risk-matrix.md`；`level=high` 联动 Gate 2 优先审、Implement 优先验、加强 Review。
- **可选 `estimated_complexity` 字段**：取值 `L1`–`L5`（Agent 可执行性分级），指导 Implement 阶段 Agent 调度策略、上下文预算分配与风险预判；与 `risk`（威胁轴）、`estimate`（工作量轴，本模板不含）三者正交，互不可替。Dev 起草、Tech Lead 在 Gate 2 确认。
