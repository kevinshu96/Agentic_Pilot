# HSD 13014834125 总结

## 基本信息
- id: 13014834125
- title: [LNL][Resiliency] ESE does not report corrupted partition when TBT/NPHY are corrupted in the $CPD header
- tenant: ipu_process.feature
- status: complete
- owner: ysharvit
- priority: 2-high
- family: Intel Platform Update (IPU)
- release: IPU 2026.3
- scope_type: driver
- fw_sw_ingredient: ESE
- customers_requesting: dell
- submitted_date: 2026-03-08
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/13014834125

## Problem Overview
当 **TBT/NPHY** 分区的 **`$CPD` header** 发生损坏时，固件处理不健壮，将损坏误判为"分区不存在"，而不是正确报告"分区损坏"。

该问题由 **Dell 在 PTL resiliency testing** 中发现，PTL 上已处理且 Dell 确认修复。由于 PTL/ARL/MTL/LNL 使用相同固件分支，多平台可能受影响（后经评论澄清 MTL Mobile 由 CSME 处理，不受影响）。

业务影响：不修复导致客户修复一致性问题，且可能导致 TBT/NPHY 功能不可用甚至系统挂起。

## Root Cause
- **错误处理逻辑未覆盖损坏场景**：读取 partition manifest 失败时，固件将其统一处理为"分区不存在"，而未区分"分区存在但 header/manifest 损坏"的情况
- **验证覆盖不足**：该场景未在测试套件中覆盖；潜在 corruption 位置 150+，难以通过单一用例全覆盖

## Fix Description
在分区读取/解析逻辑中加入区分机制：
- 将"读取 partition manifest 失败"与"partition 不存在"两类情况正确区分
- ESE 能正确报告"分区损坏/异常"而不是误报"不存在"

交付信息：
- 修复已作为 **IPU 2026.3** 的一部分交付，release drop **WW11.4**（早期目标为 WW14，评论确认提前交付）
- 验证：无单一专用测试用例，使用代表性用例覆盖

## Comments 关键讨论（按时间顺序）
1. **ogiladi**：影响平台为 LNL 与 MTL（M/P）；修复纳入 **IPU26.3**，与 TEP RCR 一起交付（目标 WW14）
2. **mhchuapr**：AR 行动项 — 确认测试用例是否已加入套件；请 Dell 在 LNL/ARL/MTL 上验证；确认受影响平台范围
3. **ogiladi**（状态更新）：修复已在 IPU26.3 release drop **WW11.4** 交付；MTL/ARL Mobile 上由 CSME 处理，该问题与 MTL 不相关；ESE 从 ARL-S/LNL 开始加载这些 IP

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论来自主票据 HSD 13014834125 的 description 与 comments 字段

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#13014834125_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。


