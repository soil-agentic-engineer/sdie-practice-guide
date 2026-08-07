# 版本变化说明 / CHANGELOG（文档版本化配套）

> 引用来源：
> - **Keep a Changelog**（Olivier Lacan, 2016，keepachangelog.com）— SemVer 生态的标准 CHANGELOG 惯例。
> - **IEEE 828-2012**《Configuration Management in Systems and Software Engineering》— 变更控制须"维护配置变更和审计结果的记录"。
> - 工作空间衔接：`0-References/git-conventions.md`（Conventional Commits 便于自动生成 CHANGELOG）。
> 在 SDIE 中引用位置：`SDIE-Spec-Guide.md` §6.5（版本历史段落，方案 A）。
> 用途：解释"为何在文档层引入版本变化说明"，支撑 version 概念可追溯。

## 定义
版本变化说明（CHANGELOG）= 把"版本号"与"该版本的具体变化"钉合的人类可读记录。每条含：版本 / 日期 / 变更摘要 / 关联决策（可选）。

## 核心要素（引自 Keep a Changelog + IEEE 828）
- **版本号 + 变化一一对应**：版本一经发布不可改（SemVer 2.0.0）；变化说明解释"为何递增"。无变化说明的版本号只是无语义计数器。
- **结构化条目**：Keep a Changelog 建议 `Added / Changed / Deprecated / Removed / Fixed` 分类；SDIE 采用更轻的"版本/日期/摘要"表，降低维护成本。
- **变更可审计**：IEEE 828 要求变更控制过程记录配置变更与审计结果，CHANGELOG 是其文档层落地。
- **与提交历史互补**：Git Conventional Commits 可自动生成 CHANGELOG（见 `git-conventions.md`），但文档消费者（PM/SME/QA）未必翻 Git 历史，故文档层须自含变化说明。

## 在 SDIE 中的用法（方案 A）
- SDIE 采用**轻量版**：每篇基线化产出物正文末尾维护 `## 版本历史` 段落（Markdown 表格 / YAML 注释），**不进 frontmatter**（七字段不变，避免膨胀）。
- 每条：`| 版本 | 日期 | 变更摘要 | 关联 ADR |`，与 `SDIE-Spec-Guide.md` §6.2 状态机、§6.3 SemVer 协同。
- 重大变更挂 ADR（§6.4），版本历史条目链接 ADR，形成"版本号（机器可校验）+ 变化说明（人类/审计）+ ADR（决策溯源）"闭环。
- YAML 型模板（如 `TASK-*.yaml` / `Decomposition-template.yml`）用注释块承载同等版本历史。

## 权威出处
- Keep a Changelog: https://keepachangelog.com/ (Olivier Lacan, 2016)
- SemVer 2.0.0: https://semver.org/ （CHANGELOG 的版本号语义来源）
- IEEE 828-2012 §3 变更控制 / 配置项状态记账（配置变更记录要求）
- 工作空间：`0-References/git-conventions.md`（Conventional Commits → CHANGELOG）
