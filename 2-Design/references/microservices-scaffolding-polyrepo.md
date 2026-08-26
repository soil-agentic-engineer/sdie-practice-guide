---
id: DOC-MS-DIRPV-001
title: 微服务架构目录结构设计方案（范式 C·垂直切片：多 git 仓 + 前后端同仓）
status: draft
phase: Design
owner: Tech Lead / 架构负责人
related_docs:
  - SDIE-RACI-Matrix.md
  - SDIE-Design-Guide.md
last_updated: 2026-08-26
---

# 微服务架构目录结构 · 范式 C·垂直切片

> 本文是范式 A（Monorepo）、范式 D（特性/发布优先）的并列替代方案。
> **核心约束（用户给定）**：① 微服务架构；② **多 git 仓库管理（Polyrepo）**；③ **每个仓内前端 + 后端同仓**（垂直切片）。
> 结论性组织：系统级拆分多个 git 仓，**每个仓 = 一个业务域（bounded context），内含该域的前端应用 + 后端微服务 + 本域 SDIE 制品**；另设 **`meta-sdie/` 治理仓** 统一承载跨域 SDIE 纪律（RACI / 全局 ADR / Gate 脚本 / 契约注册 / 全量索引）。

---

## 0. 与既有范式对照

| 维度 | 范式 A Monorepo | 范式 D 特性/发布 | **本文 C·垂直切片** |
|------|----------------|----------------|---------------------|
| 仓数量 | 单仓 | 单仓 | **多仓（每域一仓 + 1 治理仓）** |
| 前端/后端位置 | 同仓 `apps/`+`services/` | 同仓，按 feature 切片 | **同仓（每域仓内 `apps/`+`services/`）** |
| SDIE 制品 | 全局 `docs/` | `features/` 内 | **每域仓内 `docs/` + 治理仓全局层** |
| 迭代支持 | `iterations/` | `releases/`+`features/` | **每域仓自管 SDIE 周期 + 治理仓 `releases/<train>/` 编排** |
| 跨服务追溯 | 天然（同仓） | `docs/INDEX` | **治理仓 `INDEX` + 契约注册 + 发布列车 MANIFEST** |

> **不变项**：frontmatter 七字段、`status` 状态机、RACI（A 属人）、Gate 1–4、不可委托 ①–⑩、Git 分支/标签约定。

---

## 1. 设计原则

1. **垂直切片自治**：每域仓自含其前端+后端+SDIE 制品，团队可独立开发、独立 `baseline`、独立发布。
2. **治理集中、执行下沉**：`meta-sdie/` 统一 RACI / 全局 ADR / Gate 阈值 / 模板 / 契约口径；各域仓引用并细化，执行在域内完成。
3. **契约跨仓单一真源**：服务间契约（API/Proto）在治理仓 `packages/contracts/` 注册，各域仓消费，避免隐式耦合。
4. **发布列车编排迭代**：系统级迭代 = 一次"发布列车"，由治理仓 `releases/<train>/` 圈定各域仓的版本组合与 Gate 4 证据。
5. **模板双向同步**：四阶段模板权威副本在治理仓 `.sdie/templates/`，各域仓按需复制本地副本（复制而非软链，规避跨仓依赖）。

---

## 2. 整体多仓布局（org 级别）

```
<org>/                                          # 组织根（多 git 仓）
├── meta-sdie/                                  # ★ 治理仓（唯一跨域 SDIE 纪律载体）
│   ├── README.md                               # 多仓协作约定入口
│   ├── AGENTS.md                               # 全局 Agent 边界 / RACI 摘要 / 不可委托清单
│   ├── RACI.md                                 # 本项目 RACI 摘要（引自 SDIE-RACI-Matrix）
│   ├── .sdie/
│   │   ├── config.yaml                         # 全局 Gate 1-4 阈值 / 状态机 / SemVer 策略
│   │   ├── templates/                          # 四阶段模板权威副本
│   │   └── scripts/                            # frontmatter/status/gate 校验脚本（各域 CI 复用）
│   ├── docs/
│   │   ├── adr/                                # 跨域全局 ADR（红线②）
│   │   └── INDEX.md                            # ★ 全量域仓 + 服务/前端 + ADR 索引（跨仓追溯入口；方法溯源见 sdie-practice-guide/0-References/README.md）
│   ├── packages/
│   │   └── contracts/                          # ★ 跨域契约注册（OpenAPI/Protobuf 单一真源）
│   └── releases/                               # ★ 发布列车（系统级迭代编排）
│       ├── train-v1.0.0/
│       │   ├── MANIFEST.md                     # 各域仓→版本映射（domain-checkout@v1.2.0 …）
│       │   ├── gate-1.md … gate-4.md           # 系统级四道门禁核查（聚合各域证据）
│       │   └── RELEASE-NOTES.md
│       └── train-v1.1.0/ ...
│
├── domain-checkout/                            # ★ 仓1：结算域（前端+后端同仓）
├── domain-catalog/                             # ★ 仓2：商品域
├── domain-identity/                            # ★ 仓3：身份域
└── domain-payment/                             # ★ 仓4：支付域
```

> 每个 `domain-*` 是**独立的 git 仓库**，结构见 §3。

---

## 3. 单仓内部结构（domain-checkout 示例：前端+后端同仓）

```
domain-checkout/                                # 独立 git 仓（一个业务域）
├── README.md                                   # 域说明：职责边界 / 含哪些服务与前端 / 依赖契约
├── AGENTS.md                                   # 继承 meta 全局约定，细化本域 Agent 边界与 RACI
├── .sdie/                                      # 薄治理层（引用 meta 模板副本 + 本域脚本）
│   └── templates/                              # 从 meta 复制的四阶段模板（本地副本）
├── docs/                                       # ★ 本域 SDIE AI Coding 制品（四阶段，引用库统一在 meta，域仓零引用）
│   ├── 1-Spec/
│   │   ├── _templates/  PRD/  story-map/  acceptance_criteria/  task-specs/
│   ├── 2-Design/
│   │   ├── decomposition/  test-strategy/  context-injection/  security-design/
│   ├── 2-Design/adr/                          # 本域 ADR（红线②，跨域的提 meta）
│   ├── 3-Implement/
│   │   ├── behavior-checklist/  case-delta/  review-checklist/  gate3-checklist/
│   ├── 4-Evaluation/
│   │   ├── quality-dashboard/  release-decision/  adversarial-report/
│   │   ├── business-value/  eval-metrics/  retrospective/
│   └── iterations/                            # 本域迭代归档（每轮增量视图，MANIFEST 索引）
│       └── iter-001/  (ITER-PLAN / MANIFEST / gates/ / RELEASE-NOTES / RETRO)
│
├── apps/                                       # ★ 本域前端应用（与后端同仓）
│   └── checkout-web/                           # 结算域 Web 前端（React/Vue…）
│       ├── src/  tests/  e2e/  package.json  README.md
├── services/                                   # ★ 本域后端微服务（一域可含多服务）
│   ├── checkout-api/                           # 结算 API 服务
│   │   ├── src/  tests/  Dockerfile  service.yaml  README.md
│   └── checkout-worker/                        # 结算异步 Worker（示例第二个服务）
├── packages/                                   # 本域内部共享库（域级，非跨域）
│   └── shared-types/
├── e2e/features/                               # 本域跨服务验收测试（Gherkin 映射 acceptance_criteria）
├── infra/                                      # 本域部署（docker-compose / k8s / terraform）
├── scripts/  .github/ (或 ci/)  .workbuddy/
```

> **关键点**：前端 `apps/checkout-web/` 与后端 `services/checkout-api/` 同在 `domain-checkout/` 仓内，满足"前后端同一仓库"。跨域调用一律经 `meta-sdie/packages/contracts/` 契约，不直连他域代码。

---

## 4. 跨仓 SDIE 治理如何运作

| 治理项 | 落点 | 机制 |
|--------|------|------|
| 全局 RACI / 不可委托 | `meta-sdie/RACI.md` + `AGENTS.md` | 各域仓 `AGENTS.md` 继承并细化，不另起炉灶 |
| 门禁阈值（红线③） | `meta-sdie/.sdie/config.yaml` | 各域 CI 复用同一份阈值，保证全局一致 |
| 模板权威副本 | `meta-sdie/.sdie/templates/` | 各域复制本地副本（避免跨仓软链脆弱） |
| 契约单一真源 | `meta-sdie/packages/contracts/` | 域仓消费契约生成 client/stub；契约变更走全局 ADR |
| 全局 ADR | `meta-sdie/docs/adr/` | 跨域架构决策；域级 ADR 在域内，重要者提级 |
| 跨仓追溯 | `meta-sdie/docs/INDEX.md` | 列出全部域仓/服务/前端/ADR 及当前版本；方法溯源统一指针 → `sdie-practice-guide/0-References/README.md`（域仓零引用库，避免双源漂移） |

---

## 5. 迭代模型（Polyrepo 下的迭代支持）

系统级迭代 = **发布列车（release train）**，双层支持：

1. **域内 SDIE 周期**：各域仓在 `docs/1-Spec…4-Eval` 走完 SDIE 全周期，用 `docs/iterations/iter-NNN/` 归档本轮增量；分支 `spec/<f>`→`design/<f>`→`feature/<id>`→`release/<v>`，本域打 `vX.Y.Z` 标签。
2. **系统级发布列车**：治理仓 `releases/train-vX.Y.Z/MANIFEST.md` 圈定"本次列车各域仓的版本组合"（如 `domain-checkout@v1.2.0, domain-catalog@v1.0.3`），聚合各域 Gate 4 证据，存 `gate-1..4.md` 与 `RELEASE-NOTES.md`。
3. **版本归属**：域仓用自身 SemVer；列车版本标识"组合发布"。跨域不兼容变更须先在 `meta-sdie/docs/adr/` 定案。
4. **闭环收尾**：每列车结束沉淀 MANIFEST + Gate 4 + RELEASE-NOTES + 系统级 RETRO（可置于治理仓 `releases/train-*/RETRO.md`）。

> 该机制让"多仓"既不丢失迭代能力，又保持每域独立演进；迭代可见性由 `meta-sdie/docs/INDEX.md` 与列车 MANIFEST 提供。

---

## 6. 命名约定

| 对象 | 规则 | 示例 |
|------|------|------|
| 治理仓 | `meta-sdie` | — |
| 域仓 | `domain-<bounded-context>` | `domain-checkout` |
| 域内前端 | `apps/<app>` | `checkout-web` |
| 域内服务 | `services/<svc>` | `checkout-api` |
| Feature / Task / ADR / 迭代 | 同范式 A（见原文档 §5） | `FEAT-CART-001` / `TASK-CART-001.yaml` / `ADR-0007-*` / `iter-001` |
| 发布列车 | `train-vMAJOR.MINOR.PATCH` | `train-v1.0.0` |

---

## 7. SDIE 门禁 / 不可委托映射

| 要素 | 落点 |
|------|------|
| Gate 1（PM） | 域仓 `docs/1-Spec/task-specs/` + 域仓 `iterations/.../gates/gate-1.md` |
| Gate 2（Tech Lead） | 域仓 `docs/2-Design/decomposition/` + `adr/` |
| Gate 3（QA→Reviewer→PO） | 域仓 `docs/3-Implement/gate3-checklist/` |
| Gate 4（QA） | 域仓 `docs/4-Evaluation/release-decision/` + 治理仓 `releases/train-*/gate-4.md` |
| 红线① 需求/验收语义 | 域仓 `docs/1-Spec/` |
| 红线② 架构/ADR | 域仓 `adr/` + 治理仓 `docs/adr/`（跨域） |
| 红线③ 门禁阈值 | 治理仓 `.sdie/config.yaml`（全局统一） |
| 红线⑤ 安全 | 域仓 `docs/2-Design/security-design/` + `4-Evaluation/adversarial-report/` |
| 红线⑦ 收货合并 | Git `release/<version>` + PO 签字（域内）+ 列车 MANIFEST（系统级） |
| 红线⑩ Harness | `meta-sdie/AGENTS.md` + `.sdie/` + 各域 CI |

---

## 8. 取舍与适用场景

- ✅ **适用**：多团队、强合规/独立发布、域边界清晰、希望每域技术栈/节奏自治的微服务系统。
- ✅ 隔离彻底：一域故障/重构不波及其他仓；权限、CI、发布互不影响。
- ❌ **代价**：跨域契约与门禁一致性靠治理仓 + 工具强约束，人工协调成本高于 Monorepo；跨服务追溯需主动维护 INDEX / 契约注册 / 列车 MANIFEST。
- ❌ 不适合：小团队、服务少、追求单一工具链与零协调开销的场景（选范式 A 更省）。

---

## 9. 自审（占位符 / 一致性 / 范围 / 歧义）

- **占位符**：无 `<FEATURE>` 类未填遗漏；`<org>/<domain-*>` 为部署时替换的语义占位。
- **一致性**：SDIE 七字段/状态机/Gate/红线与 `SDIE-RACI-Matrix` 及各阶段指南一致；命名沿用本仓库 `1-Spec/2-Design/...` 风格。
- **范围**：仅目录结构方案；不含具体模板正文、CI 脚本、技术栈选型（留作 ADR）。
  - **歧义**：域级 `adr/` 与治理仓 `docs/adr/` 共存——域级存本域决策，跨域决策提级治理仓，INDEX 互链避免双源漂移。**引用库同理**：方法溯源不复制进产品 org，统一指针指向权威方法论源 `sdie-practice-guide/0-References/README.md`；各域仓 `docs/` 与 `meta-sdie/docs/` 均不各自带 `0-References/`，防止方法论文献双源漂移。

---

## 10. 下一步（待人类拍板，红线②）

1. **Tech Lead 审批本结构**（文档 `status` 升 `baseline` 前需复核）。
2. 明确"域"的切分边界（哪些服务归入 `domain-checkout`），写 ADR 记录（红线②）。
3. 补 `meta-sdie/.sdie/config.yaml` 的 Gate 1–4 全局阈值；确定契约格式（OpenAPI/Protobuf）。
4. 决定域内 SDIE 制品采用"阶段式"（本文）还是"feature 式"（范式 D），保持各域一致或允许差异。

> 本文由 AI 协作生成，SDIE 事实以 `SDIE-RACI-Matrix.md` 为唯一权威基准；架构选型（红线②）、门禁阈值（红线③）等决策须由对应人类角色审定。
