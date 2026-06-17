# HSD 14027457811 总结

## 基本信息
- id: 14027457811
- title: [GNR][uCode bugeco] GNR-SP: C6 disabled, >2 AliSCM disks, heavy IO, AC pull — GPF fails to trigger
- tenant: ipu_process.feature
- status: complete
- owner: faisalal
- priority: 2-high
- family: Birch Stream Platform
- release: Birch Stream Platform GNR AP
- scope_type: bug_fix
- fw_sw_ingredient: starcode
- customers_requesting: alibaba
- submitted_date: 2026-03-20
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027457811

## Problem Overview
在 **GNR-SP** 平台上，**C6 disabled** 条件下，同一 socket 连接**超过两块 AliSCM 磁盘**并施加 **heavy IO stress** 时，执行**断 AC 电源**操作后，**GPF（Global Persistent Flush）触发失败**。

- 在 CRB 上复现率约 **~100%**
- 问题由 **Alibaba** 客户通过 HSD 15019039162 报告

## Root Cause
ADR 流程完成时间过长，导致在上述断电/重 IO 压力场景下 GPF 触发路径无法按预期完成。

进一步分析：Alibaba 用例仅需 **IO buffer flush**；ADR 中执行的 **core cache flush** 是导致时序问题的关键因素（对该客户场景属于不必要开销）。

## Fix Description
通过 **pCode + uCode 联合补丁**缩短 ADR 流程完成时间：

- **uCode 侧**：在 ADR 中**跳过 core cache flush**（仅保留 IO buffer flush）
- 具体变更：uCode 中 spend 1 PM，从 **U67f4** 跳转到 **Uaa6d**（GNR UROM）
- 关联目标（来自 HSD 15019039162）：将 GPF 流程时间降低到 **2ms 以内**（GNR UPLR1.6 PV release）

## Comments 关键讨论（按时间顺序）
1. **davidalb**：自动消息，记录从父记录 14027227516 clone
2. **davidalb**：GK Decision — 线下已达到 GK approval（per Geetha），覆盖 **UPLR5 GNR AP/SP, SRF AP/SP**
3. **davidalb**：自动消息，记录从父记录 14027234721 clone
4. **davidalb**：该 clone 已创建并获得 GK 批准：**UPLR1.6 GNR AP/SP, SRF AP/SP**

## Link HSD 信息
- 关联 HSD ID: 15019039162
- URL: https://hsdes.intel.com/appstore/article-one/#/article/15019039162
- 关联原因: 主票描述中显式引用该票为 Alibaba 问题来源/关联案例

## 信息补充说明
- Root/Fix 主体来自主票 HSD 14027457811
- 额外获取信息来自关联 HSD 15019039162：
  - Problem Overview 补充：同一场景（GNR-SP + C6 disabled + >2 AliSCM + heavy IO + 断 AC + GPF 不触发），复现率 ~100%，循环断上电约 10-20 次
  - Fix Description 补充：关联票记录目标为"将 GPF 流程时间降低到 2ms 以内（GNR UPLR1.6 PV release）"
  - Root Cause 补充：关联票未提供明确 root cause 字段

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#14027457811_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。


