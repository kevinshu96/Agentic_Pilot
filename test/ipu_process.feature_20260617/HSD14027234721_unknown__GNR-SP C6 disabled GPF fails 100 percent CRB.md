# HSD 14027234721 总结

## 基本信息
- id: 14027234721
- title: [GNR][uCode bugeco] GNR-SP: C6 disabled, heavy IO stress, GPF fails to trigger - failure rate 100% on CRB
- tenant: ipu_process.feature
- status: complete
- owner: btshetty
- priority: 1-critical
- family: GNR
- release: Birch Stream Platform GNR AP
- scope_type: bug
- fw_sw_ingredient: Microcode/ADR
- customers_requesting: Internal Testing
- submitted_date: 2026-03-03
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027234721

## Problem Overview
在 **GNR-SP** 平台上，**C6 disabled** 条件下，同一 socket 连接**多块 AliSCM 磁盘**（>2块）并施加 **heavy IO stress**，执行 **AC 电源断电**后，**GPF（Global Persistent Flush）触发失败**。

测试结果：**CRB 上 100% 复现率**，存储保护机制完全失效。

## Root Cause
**ADR（Asynchronous DRAM Refresh）流程完成时间过长**导致：
- ADR 序列中的 **core cache flush 步骤**在 C6 disabled 且高 IO load 竞争环境下耗时过长
- 在 AC power 突然断开时，ADR 未能在规定时间内完成 flush 操作
- 导致 GPF 无法正确触发，存储一致性无法保证

深层机制：C6 禁用后，CPU 无法进入深睡眠，频繁在高频运行；IO stress 导致系统总线竞争剧增，ADR flow 在频繁内存冲刷中严重延迟。

## Fix Description
通过 **PCode + UCode 联合补丁**优化 ADR 流程：
- **UCode 侧**：**跳过 core cache flush**（仅保留 IO buffer flush）
- 修改 GNR UROM 中 ADR 实现：从 `U67f4` 直接跳转到 `Uaa6d`
- 将 ADR flow 完成时间缩减，确保在 power loss 窗口内及时完成

背景说明：通过与 Alibaba（原问题报告方）沟通，发现其使用场景仅需 **IO buffer flush**，不需要 core cache flush；该优化既解决时延问题，又满足客户需求。

交付信息：修复纳入 **UPLR1.6 GNR AP/SP, SRF AP/SP**。

## Comments 关键讨论
- 存储保护可靠性关键问题
- 影响所有配置多块 NVMe/AliSCM 且使用 ADR 的系统
- 内部测试中 100% 复现

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论来自主票据 HSD 14027234721
- 该票为 parent record，衍生了多个 child UPLR-specific tickets

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#14027234721_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。
