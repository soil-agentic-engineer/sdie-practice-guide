# Docs-as-Code（YAML 元数据块载体）

> 引用来源：静态站点生成器生态（Jekyll / Hugo / MkDocs / Docusaurus）通用实践
> 在 SDIE 中引用位置：SDIE-Spec-Guide §6.1（元信息七字段的载体来源）
> 用途：解释 SDIE 文档为何用 YAML 块挂元数据。

## 定义
Docs-as-Code = 用写代码的方式写文档（版本化、纯文本、可构建）。其中 **YAML frontmatter** 是 Jekyll 引入、被 Hugo / MkDocs / Docusaurus 广泛采用的元数据块惯例：文档顶部以 `---` 包裹的 YAML 承载结构化字段。

## 核心要素
- 文档即数据：元数据与正文分离，机器可解析。
- 与 Git 同 lifecycle，便于 diff / 审查 / CI。

## 在 SDIE 中的用法
- SDIE 当前约定：每篇阶段产出物在 **第 1 节「元信息（meta）」以 YAML 代码块**挂七字段（id / title / status / phase / owner / related_docs / last_updated）；机器规格型文档（YAML）则将七字段挂在 **`meta:` 块下**，其余业务字段归入语义分组键（如 `spec:` / `plan:` / `checklist:` / `metrics:`）。该约定由 `SDIE-Analysis.md` §8.1 定义，frontmatter 仅是可选历史形态。
- **重要区分**：七字段本身是 SDIE「空间已定义」约定（非外部方法论）；Docs-as-Code 只是其技术载体。纯 YAML 模板（如 decomposition.template.yml）去掉 `---` 围栏，整篇即 YAML，并以 `meta:` 顶层键承载七字段。

## 权威出处
- Jekyll (Tom Preston-Werner, 2008) 首创 frontmatter 惯例。
- Hugo / MkDocs / Docusaurus 文档工程生态。
