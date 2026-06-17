# HSD 18044357768 总结

## 基本信息
- id: 18044357768
- title: [GNR][BMC][Dell/17G] To support parameter 2 as 0xFF for SPDM commands (Con't Of 01001982)
- tenant: ipu_process.feature
- status: rejected
- owner: mgleba
- priority: 2-high
- family: Birch Stream Platform
- release: Birch Stream Platform GNR AP
- scope_type: feature_request
- fw_sw_ingredient: starcode
- customers_requesting: dell
- submitted_date: 2026-04-03
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/18044357768

## Problem Overview
在 **Dell 17G（BHS/GNR）** 平台上，SPDM `GET_MEASUREMENTS` 当 **Parameter 2=0xFF**（一次性请求所有 measurements）时，聚合响应超过 **I3C/MCTP 传输大小限制（约 512B）**，被 I3C_MNG 静默丢弃/截断，导致 "Get All Signed Measurements" 聚合签名校验失败。逐个 index 请求时签名校验正常。

相比对照：**CWF** 平台上同等 SPDM 功能已集成且工作正常。

## Root Cause
I3C/MCTP 传输链路对大于约 512B 的消息有 silent drop 限制。SPDM `GET_MEASUREMENTS(0xFF)` 单次返回全量 measurements，响应超过该限制，导致传输层截断，聚合签名因报文不完整而失败。

## Fix Description
该 ticket 最终 **Rejected（wont_do）**，原因是被更大范围的完整修复覆盖，追踪在 **HSD 14025499978**。

本 ticket 中讨论的修复方向：
- 修改 MCTP 消息发送行为（SPDM-only）：由 task 按间隔分片发送（每隔几毫秒一个 fragment），让 I3C 有时间处理
- 严格限定变更范围在 SPDM/GET_MEASUREMENTS(0xFF)，不引入全局 rate limiting 复杂度
- 会议讨论倾向 UPLR6 做完整 streaming fix 而非 UPLR5 SPDM-only 方案

## Comments 关键讨论（按时间顺序）
1. **rexkuo**（路由后）：Dell Top-7 诉求——BHS/OKS 支持 SPDM `0xFF` 一次取全量；BHS 上 I3C_MNG >512B 存在 silent drop；OKS 上类似限制已部分修复但 DMR ~26 个 measurement index 仍可超 1KB
2. **rexkuo**：关联内部工作项 HSD 18040462982（特定消息需 FW 拆分为 64B payload）
3. **rexkuo**：与客户澄清后，Dell 决定**不将该需求纳入 Dell 17G(BHS) scope**，改为要求 OKS 支持
4. **自动消息**：关联 parent records 14027265097、18044357742、14025499978
5. **davidalb**（会议纪要）：讨论 Dell CPU attestation / SPDM over I3C >512B 场景；UPLR5 做 SPDM-only vs UPLR6 全面 streaming fix；开发估计 1–2 周 + 验证 3 周
6. **kamanna**：客户可等 UPLR6 完整方案，不需要两套并行实现
7. **mkaplins**：最终 Rejected，需求追踪至 HSD 14025499978

## Link HSD 信息
- 关联 HSD ID: 14025499978
- URL: https://hsdes.intel.com/appstore/article-one/#/article/14025499978
- 关联原因: 该需求最终被此更大范围修复覆盖并 rejected（来自评论 mkaplins）

## 信息补充说明
- 本文结论均来自主票据 HSD 18044357768
- Link HSD 14025499978 为实际承接完整修复的票据，来源于评论显式引用
