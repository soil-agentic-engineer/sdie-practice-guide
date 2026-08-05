---
id: SEC-DESIGN-<FEATURE>
title: <功能>安全设计点
status: draft
phase: Design
owner: security-<名>       # 安全 / 红队（C，判定权 ⑤）
related_docs:
  - ADR-DESIGN-000
last_updated: 2026-08-06
---

# <功能>安全设计点

> **作者**：安全/红队（C，咨询）；**策略判定与合规结论不可委托（⑤）**。
> 本文件在 Design 阶段预埋扫描关注项，供 Implement 扫描规则与 Evaluation 对抗演练消费。

## 认证 / 授权
- 加购接口需登录态 + 资源归属校验（越权 ⑤ 红线）。
- 禁止 Agent 修改安全配置（⑩ Harness 维护归 Dev+TL）。

## 数据保护
- 购物车含 PII 时加密存储；日志脱敏。
- 测试数据禁止携带生产 PII。

## 红队重点攻击面（标注供 Evaluation 演练）
- 提示注入篡改加购数量
- 越权读取他人购物车
- 并发超卖（边界）

## 合规关注
- 数据驻留 / 审计日志要求（若适用）。
- 合规结论最终由安全/红队出具（⑤）。
