# HSD 16030147687 总结

## 基本信息
- id: 16030147687
- title: [GNRSP][2DPC 6000]4H 3DS is not POR, hence limit the frequency to 5200
- tenant: ipu_process.feature
- status: complete
- owner: sgchetha
- priority: 2-high
- family: Birch Stream Platform
- release: Birch Stream Platform GNR SP
- scope_type: feature_request
- fw_sw_ingredient: BIOS
- customers_requesting: dell
- submitted_date: 2026-03-23
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/16030147687

## Problem Overview
在 **GNR SP 2DPC** 配置下，当两个插槽均检测到 **4H 3DS** 时，需要 BIOS 将内存频率限制到 **5200 MT/s**。

背景：4H 3DS 在 2DPC 6000 配置下不属于 POR（不在产品支持基线内），通过限频策略规避潜在稳定性风险。该变更为 **Dell** 客户请求，纳入 **UPLR4**。

## Root Cause
No Info（主票未提供明确技术根因；当前仅能确认 4H 3DS 在该配置下不属于 POR，需限频保障稳定性）

## Fix Description
BIOS 中加入频率限制策略：当 GNR SP 2DPC 场景检测到双槽均为 4H 3DS 时，将频率上限设为 5200 MT/s。

实现载体：
- 对应 PR：`firmware.boot.uefi.iafw.intel` [PR#137893](https://github.com/intel-restricted/firmware.boot.uefi.iafw.intel/pull/137893)
- GK 状态：**GK Approved UPLR GNR SP**（已通过 Gatekeeper 批准）

## Comments 关键讨论（按时间顺序）
1. **mmroquec**：提供对应实现 PR 链接（GitHub intel-restricted PR#137893）
2. **davidalb**：GK Decision — GK Approved UPLR GNR SP

## Link HSD 信息
- 关联 HSD ID: 14027323128
- URL: https://hsdes.intel.com/appstore/article-one/#/article/14027323128
- 关联原因: 主票 issue_links 中指向 Central Firmware 侧同需求关联条目

## 信息补充说明
- 主票 Root Cause 字段为空，证据不足（来源：HSD 16030147687）
- 额外获取信息来自关联 HSD 14027323128：
  - Problem Overview 补充：关联票同样给出"当双槽检测到 4H 3DS 时，将 GNR SP 2DPC 频率限制到 5200"需求
  - Root Cause 补充：关联票 root_cause 字段为空
  - Fix Description 补充：关联票 solution_outline 字段为空
