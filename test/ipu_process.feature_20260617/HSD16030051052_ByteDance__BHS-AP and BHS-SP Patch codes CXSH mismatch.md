# HSD 16030051052 总结

## 基本信息
- id: 16030051052
- title: [MEM][ByteDance][BHS-AP and BHS-SP][Anji] Some patch codes in UPLR3 release mismatch with CXSH required
- tenant: ipu_process.feature
- status: complete
- owner: sgchetha
- priority: 1-critical
- family: GNR
- release: Birch Stream Platform GNR SP
- scope_type: bug
- fw_sw_ingredient: Memory Training/MRC
- customers_requesting: ByteDance
- submitted_date: 2026-03-10
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/16030051052

## Problem Overview
在 **GNR BHS-AP/BHS-SP** 平台上，**UPLR3 release** 中的 **patch codes** 与 **CXSH 内存支持要求不匹配**，导致：
- **MRW（MCDRAM Read Write）数据设置错误**
- **CXSH-specific 内存配置验证失败**
- 内存训练中断或参数设置不正确

该问题为 **ByteDance** 客户 **1-critical** 级别，直接阻塞 production 部署。

## Root Cause
**UPLR3 release 中的 patch codes** 与 **CXSH（某内存子系统）requirement** 存在**不一致**：
- MRC 参数配置与 CXSH 预期不符
- patch code 版本号或逻辑与 CXSH spec 脱节
- 导致内存训练过程中触发错误或应用错误的配置

## Fix Description
修复方案包含两部分：
1. **更新 patch codes** 使其与 CXSH requirements 对齐
   - 同步最新的 CXSH spec 规范
   - 修正 MRC parameter mapping
   - 验证 patch code 逻辑正确性

2. **进行全面验证测试**
   - 在 BHS-AP/BHS-SP 上执行内存训练测试
   - 涵盖所有受影响的 CXSH memory 配置
   - 确保无 training failure 或 parameter mismatch

预期结果：ByteDance 能在 GNR 平台上正确启用与使用 CXSH 内存。

## Comments 关键讨论
- 关键 CXSH 内存启用问题
- 直接影响 ByteDance production deployment timeline

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论来自主票据 HSD 16030051052

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#16030051052_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。
