# HSD 14027558754 总结

## 基本信息
- id: 14027558754
- title: TDX 1.5.32 - Incorrect handling of handoff data retrieval for HV = 0
- tenant: ipu_process.feature
- status: complete
- owner: thorodi
- priority: 2-high
- family: Intel Platform Update (IPU)
- release: IPU 2026.3
- scope_type: driver
- fw_sw_ingredient: TDX
- customers_requesting: (空)
- submitted_date: 2026-04-01
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027558754

## Problem Overview
在 **TD-Preserving warm update** 时，从 **TDX module 1.5.20（IPU 25.4）** 升级到 **1.5.32（IPU 26.3）** 失败。

触发条件：**仅在 Dynamic PAMT 被启用且旧模块 HV=0 时**，升级到 1.5.32 及更新版本会导致 TDX 功能 brick。

对比：1.5.24 及更新版本（HV=1）不受影响。

## Root Cause
`preserving.c` 中 `retrieve_handoff_data` 在 **`prev_hv == 0`** 分支使用了包含"connect fields"的错误格式（这些字段在 1.5 modules 中不存在），导致 `dynamic_pamt_enabled` 按错误 offset 被访问，造成 warm update 路径失败及 TDX brick。

## Fix Description
修正 `preserving.c` / `retrieve_handoff_data` 对 `prev_hv == 0` handoff data 的格式解析，消除 `dynamic_pamt_enabled` 错误 offset 读取。

状态：**修复已完成**，ingredient level validation 正在 re-spin，预计 **ww14.5** 完成。

## Comments 关键讨论
1. **tlgamby**：PV 验证中发现该 bug；该问题**仅在 1.5.20→1.5.32 之间且启用 Dynamic PAMT 时**触发；正在做紧急修复，请求批准将修复集成到 IPU 26.3 PV cycle

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论均来自主票据 HSD 14027558754

## 关联日志下载信息
- 日志下载状态: 已成功下载
- 日志文件命名规则: HSD#14027558754_<原文件名>
- 已下载的日志文件:
  - HSD#14027558754_binary_13014971688.bin (192.1 KB)
  - HSD#14027558754_binary_13014971689.bin (25.58 KB)
- 备注: 日志文件已保存至同目录，可直接查阅。


