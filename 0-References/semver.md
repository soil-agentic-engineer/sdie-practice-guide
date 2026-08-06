# SemVer 2.0.0 — 语义化版本

> 引用来源：SemVer 官方规范 https://semver.org/lang/zh-CN/ （作者 Tom Preston-Werner，初始版本 2013）
> 在 SDIE 中引用位置：各阶段指南 §7「版本化」、README「通用约定」、frontmatter `last_updated` 配套
> 用途：为 SDIE 所有阶段产出物（PRD / TASK / DECOMP / ADR / 报告）提供统一的版本号语义。

## 定义
语义化版本（Semantic Versioning）通过 `MAJOR.MINOR.PATCH` 三段数字表达版本含义：
- **MAJOR**：做了不兼容的 API 变更（破坏性变更）
- **MINOR**：做了向下兼容的功能性新增
- **PATCH**：做了向下兼容的问题修正

附加标记：预发布版本用 `-alpha / -beta`（如 `1.0.0-alpha.1`），构建元数据用 `+build`。

## 核心要素
- 版本号递增规则：仅当 MAJOR 递增时允许 MINOR / PATCH 归零，以此类推。
- `0.y.z` 视为初始开发阶段，API 不稳定。
- 必须声明公共 API 范围，版本号才具有语义。

## 在 SDIE 中的用法
- 每个阶段产出物文档随变更递增版本；`status` 状态机（draft → review → baseline → change → superseded）与 SemVer 配合：baseline 后内容不可原地涂改，需走 change 流程并递增版本。
- 文档 `last_updated` 记录修改日期，版本号记录语义级别。
- 发布打 `vX.Y.Z` 标签（见 Git 约定）。

## 权威出处
- semver.org 官方规范（中文版 https://semver.org/lang/zh-CN/）
- Preston-Werner, T. "Semantic Versioning 2.0.0", 2013.
