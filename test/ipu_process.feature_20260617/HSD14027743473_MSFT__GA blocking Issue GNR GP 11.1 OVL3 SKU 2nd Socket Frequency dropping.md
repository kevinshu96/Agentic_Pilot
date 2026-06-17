# HSD 14027743473 总结

## 基本信息
- id: 14027743473
- title: GA blocking Issue – Intel GNR GP 11.1 OVL3 SKU - 2nd Socket Frequency dropping to 200Mhz
- tenant: ipu_process.feature
- status: complete
- owner: btshetty
- priority: 2-high
- family: GNR
- release: Birch Stream Platform GNR AP
- scope_type: bug
- fw_sw_ingredient: PCode/Thermal Control
- customers_requesting: MSFT
- submitted_date: 2026-04-24
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027743473

## Problem Overview
在 **GNR GP 11.1 OVL3 SKU** 配置下，部分节点在发生 **SEL/critical event** 后，**第二个 socket 频率突然降至 ~200MHz**。系统进入 **failsafe DVFS guardband mode**，伴随潜在的 **PROCHOT assertion** 问题，导致性能崩溃。

该问题为 **MSFT** 客户 GA 阻塞件，严重影响客户生产部署。

## Root Cause
根本原因涉及 **thermal throttling**，特别是由以下因素触发：
- **PROCHOT assertion** 配置问题
- **CPU register log failures** 导致 failsafe mode 被激活

详细机制：在特定 thermal 条件或 PROCHOT 信号下，系统错误进入 failsafe DVFS 限频，导致 socket 2 频率被锁定在约 200MHz。

## Fix Description
建议解决方案包含以下步骤：
- 验证 **PROCHOT 引脚和配置**是否正确
- **禁用 CC6** 以防止 C-state 转换引发的 thermal 敏感状态
- 执行 **PECI 命令**获取实时诊断信息（温度、PROCHOT 状态、core 频率等）
- 优化 failsafe mode 的触发阈值与恢复机制（需支持无需 reboot 的动态恢复）

## Comments 关键讨论
- GA 阻塞件，影响 MSFT 生产部署
- 需要能够在不 reboot 的情况下恢复正常运行

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论来自主票据 HSD 14027743473

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#14027743473_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。
