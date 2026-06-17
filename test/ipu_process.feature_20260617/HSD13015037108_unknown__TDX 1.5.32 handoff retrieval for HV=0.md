# HSD 13015037108 总结

## 基本信息
- id: 13015037108
- title: TDX 1.5.32 - Incorrect handling of handoff data retrieval for HV = 0
- tenant: ipu_process.feature
- status: complete
- owner: thorodi
- priority: 2-high
- family: Birch Stream Platform
- release: Birch Stream Platform SRF AP
- scope_type: bug_fix
- fw_sw_ingredient: TDX
- customers_requesting: (空)
- submitted_date: 2026-04-01
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/13015037108

## Problem Overview
在进行 TD-Preserving warm update 时，从 TDX 1.5.20（IPU 25.4）升级到 1.5.32（IPU 26.3）会失败。

受影响场景：旧版本模块 HV=0 且 dynamic PAMT enabled（例如 1.5.20），升级到 1.5.32 及更新版本，可能导致 TDX 功能被 brick（不可用）。

对比：1.5.24 及更新版本因 HV=1，不受该问题影响。

背景：TDX Validation 团队使用了包含 Dynamic PAMT 支持的非 BKC Linux stack 进行端到端验证，与 IPU PV 团队环境不一致，该环境下才能触发该路径。

## Root Cause
preserving.c 中的 retrieve_handoff_data 在处理 prev_hv == 0 的 handoff data 时，使用了包含 connect fields 的格式，但这些字段在 1.5 modules 中并不存在。导致 dynamic_pamt_enabled 标志位被以错误 offset 访问，进而在升级路径中触发失败并造成 TDX 功能不可用。

## Fix Description
修正 retrieve_handoff_data 在 prev_hv == 0 场景下的 handoff data 格式解析，避免对 dynamic_pamt_enabled 的错误 offset 访问。

状态：修复已完成，ingredient level validation 正在 re-spin，预计 ww14.5 完成。

## Comments 关键讨论（按时间顺序）
1. nowickir：提到 HSD 18043882352（关联 link），通过 link tab 添加
2. tlgamby（自动消息）：记录从父记录 13014971690 clone
3. sanikere：信息来自 platform validation
4. davidalb：GK Approval Granted during ww14.4 PXT meeting — GK Approval UPR4 SRF AP

## Link HSD 信息
- 关联 HSD ID: 18043882352
- URL: https://hsdes.intel.com/appstore/article-one/#/article/18043882352
- 关联原因: 评论中 nowickir 添加的相关 link

## 信息补充说明
- 本文结论均来自主票据 HSD 13015037108
- 关联 HSD 18043882352 来自评论文本，未展开其内容

## 关联日志下载信息
- 日志下载状态: 已成功下载
- 日志文件命名规则: HSD#13015037108_<原文件名>
- 已下载的日志文件:
  - HSD#13015037108_binary_13014971688.bin (192.1 KB)
  - HSD#13015037108_binary_13014971689.bin (25.58 KB)
- 备注: 日志文件已保存至同目录，可直接查阅。


