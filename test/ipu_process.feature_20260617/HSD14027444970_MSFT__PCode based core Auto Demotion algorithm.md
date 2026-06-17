# HSD 14027444970 总结

## 基本信息
- id: 14027444970
- title: PCode based core Auto Demotion algorithm for MSFT
- tenant: ipu_process.feature
- status: complete
- owner: btshetty
- priority: 2-high
- family: GNR
- release: Birch Stream Platform GNR AP
- scope_type: feature
- fw_sw_ingredient: PCode/Performance
- customers_requesting: MSFT
- submitted_date: 2026-03-19
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027444970

## Problem Overview
**IPI-bench 测试结果**显示：当禁用 deep C-state 时，**Gen11.1 IPI latency 变异性**有改善。问题根源：
- GNR 平台上的 **C1 auto-demotion 策略过于激进**，导致 **latency 变异范围 19us~213us**
- 相比前代平台（如 SPR），GNR 更频繁进入深 idle state，导致 wake/interrupt latency 在低中断率场景下显著上升

影响范围：**MSFT Azure/BareMetal** 部署，对延迟敏感的应用性能产生负面影响。

## Root Cause
**GNR 的 C1 auto-demotion 策略设置过于激进**，相比前代平台：
- 更容易/更频繁进入 C1 及更深 C-state
- 在低中断频率下，wake latency 显著增加，导致 IPI 和中断响应延迟高且不稳定
- 该特性在 MSFT 工作负载（例如数据库频繁 context switch）上暴露明显

## Fix Description
优化 **C1 auto-demotion 策略**：
- 调整 demotion 触发阈值与保守性，减少不必要的深 C-state 进入
- 实施 **debug patch**验证新策略在 IPI latency、interrupt response 上的改善
- 提供 **pseudocode 参考**供 PCode team 集成到主线 firmware

预期收益：
- 降低 IPI 及 interrupt latency spike
- 提升延迟敏感应用稳定性（例如 Azure VM 间通信）

## Comments 关键讨论
- 性能变异问题直接影响 Azure/BareMetal 部署体验
- 包含方法论与伪代码参考

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论来自主票据 HSD 14027444970

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#14027444970_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。
