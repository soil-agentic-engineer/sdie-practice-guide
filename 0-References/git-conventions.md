# Git / Conventional Commits / trunk-based — 版本控制约定

> 引用来源：Git (Linus Torvalds)；Conventional Commits 规范 (conventionalcommits.org)；trunk-based development (Paul Hammant)
> 在 SDIE 中引用位置：各阶段指南 §7「版本化」、README「通用约定」
> 用途：规范 SDIE 各阶段产出物的分支、提交、标签策略。

## 定义
- **Git**：分布式版本控制（Torvalds, 2005）。
- **Conventional Commits**：`type(scope): description` 提交消息约定，便于自动生成 CHANGELOG 与语义化版本推断。
- **trunk-based development**：短生命周期分支、频繁合入主干，减少长期分支漂移。

## 核心要素
- 提交类型：feat / fix / docs / refactor / test / chore 等。
- 分支按阶段分：`spec/<feature>` / `design/<feature>` / `feature/<task-id>` / `release/<version>`。
- 基线 / 发布打 `vX.Y.Z` 标签（呼应 SemVer）。

## 在 SDIE 中的用法
- 每个阶段产出物按阶段前缀提交（`spec:` / `design:` / `feat:` / `eval:`）。
- baseline 后内容不可原地涂改，变更走 change 流程并递增版本、提交带 `change:` 前缀。

## 权威出处
- conventionalcommits.org
- Hammant, P. "Trunk Based Development".
- Git 官方文档 git-scm.com
