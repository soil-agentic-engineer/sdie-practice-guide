# Docs-as-Code（YAML frontmatter 载体）

> 引用来源：静态站点生成器生态（Jekyll / Hugo / MkDocs / Docusaurus）通用实践
> 在 SDIE 中引用位置：SDIE-Spec-Guide §7.1（frontmatter 七字段的载体来源）
> 用途：解释 SDIE 文档为何用 YAML frontmatter 挂元数据块。

## 定义
Docs-as-Code = 用写代码的方式写文档（版本化、纯文本、可构建）。其中 **YAML frontmatter** 是 Jekyll 引入、被 Hugo / MkDocs / Docusaurus 广泛采用的元数据块惯例：文档顶部以 `---` 包裹的 YAML 承载结构化字段。

## 核心要素
- 文档即数据：元数据与正文分离，机器可解析。
- 与 Git 同 lifecycle，便于 diff / 审查 / CI。

## 在 SDIE 中的用法
- SDIE 借用该载体：每篇阶段产出物挂 **frontmatter 七字段**（id / title / status / phase / owner / related_docs / last_updated）。
- **重要区分**：七字段本身是 SDIE「空间已定义」约定（非外部方法论）；Docs-as-Code 只是其技术载体。纯 YAML 的 Decomposition 模板（Decomposition-template.yml）进一步去掉 `---` 围栏，整篇即 YAML。

## 权威出处
- Jekyll (Tom Preston-Werner, 2008) 首创 frontmatter 惯例。
- Hugo / MkDocs / Docusaurus 文档工程生态。
