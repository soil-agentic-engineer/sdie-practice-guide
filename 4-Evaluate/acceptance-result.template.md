## 元信息（meta）

```yaml
id: ACCEPT-RESULT-<ITER>
title: <项目>-<迭代>验收结果判定
status: review
phase: Evaluation
owner: qa-<名>              # 测试架构师 / QA（R/A，⑧ 验收签收）
related_docs:
  - AC-<feature>.md              # 验收标准集（Spec，红线①）
  - BC-<TASK-ID>.yaml           # behavior-checklist（maps_to AC-N）
  - QUALITY-DASH-<ITER>         # 质量看板
last_updated: 2026-08-06
```

# 验收结果判定：<项目>-<迭代>

> **角色（7/12 版）**：QA（A，⑧ 发布放行）据本表做验收签收；PM(C) 业务价值确认另见 `business-value-confirmation.template.md`，非技术放行 A。
> **本表是 Gate 4「验收覆盖可追溯」的逐条证据**：每条 AC-N 必须有对应 behavior 与结论，禁止用"整体通过"掩盖单条失败。
> `coverage_of_ac`（eval-metrics）**应由本表 per-AC 判定派生，禁止手填**——呼应 ④·验证 红线（证据不得自报自批，异常值触发 Retrospective）。

## 逐条验收判定（per-AC verdict）

| AC 编号 | 对应 behavior | 验收语义（摘要） | 结论 | 证据链接 |
|---------|--------------|------------------|------|----------|
| AC-1 | B1 | 库存>0 加购成功，返回 200 | pass | link to CI run / test report |
| AC-2 | B2 | 库存=0 按钮禁用，无写操作 | pass | link |
| AC-3 | B3 | 并发加购不超卖，最终一致 | pass | link |
| AC-红卡（并发边界） | B3 | 并发边界由 Implement 处理并在报告体现 | pass | link |
| ... | ... | ... | blocked / fail / na | ... |

> 结论取值：`pass` / `fail` / `blocked`（阻塞，未执行） / `na`（不适用，需注明原因）。
> 任一 `fail` 或 `blocked` → 退回对应环节，不得过 Gate 4。

## 汇总
- 验收标准总数: __ | 通过: __ | 失败: __ | 阻塞: __ | 不适用: __
- `coverage_of_ac`(派生): __ （= 通过 / (总数 − na)）
- 结论: 验收全部达成 / 存在未达成项（附说明）

## 签署
- 判定人（QA）: <名> @ <日期>
- 备注: 本判定为 Gate 4 验收签收证据；技术放行以 QA 发布决策（⑧）为准。
