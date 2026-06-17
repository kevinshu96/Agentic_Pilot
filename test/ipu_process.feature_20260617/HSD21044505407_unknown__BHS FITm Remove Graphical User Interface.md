# HSD 21044505407 总结

## 基本信息
- id: 21044505407
- title: [BHS][FITm] Remove Graphical User Interface
- tenant: ipu_process.feature
- status: rejected
- owner: mmirkows
- priority: 2-high
- family: Birch Stream Platform
- release: Birch Stream Platform GNR AP
- scope_type: tbd
- fw_sw_ingredient: Tools
- customers_requesting: (空)
- submitted_date: 2026-06-02
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/21044505407

## Problem Overview
FITm（Modular FIT）目前以两种形态发布：
- **FITm CLI**：OS agnostic、Python scripts、无第三方组件
- **FITm GUI**：面向 Windows/Linux 的可执行文件，包含大量第三方组件

团队资源显著削减（约 50% 蓝牌成员削减 + CW 削减 20%），导致无法继续交付满足 SDL 与质量要求的 GUI 版本。GUI 维护还带来持续的第三方 CVE 修复负担。CLI 与 GUI 在大多数功能上等价，但移除 GUI 对服务器客户的平台配置体验（尤其是 SPI flash image 图形化配置）有显著影响。

## Root Cause
- 团队资源/人力削减导致无法持续维护 FITm GUI 并满足 SDL 与质量要求
- FITm GUI 对大量第三方组件的依赖带来持续 CVE 与测试维护负担

## Fix Description
- **建议方案（描述中）**：移除 GUI，增强 CLI（更友好的 help、补齐仅 GUI 可做的少数动作，从 Oakstream backport）
- **评论讨论后形成的过渡方案**：
  - 短期不立即移除 GUI，提供 **6 个月过渡计划**（GUI → CLI）
  - UPLR3/UPLR4 阶段 GUI 与 CLI 并行支持
  - 明确告知客户 GUI EOL/不支持声明及安全风险
  - 目标时间窗：**2026 Q3–Q4** 作为 GUI EOL 里程碑

## Comments 关键讨论（按时间顺序）
1. **mcjeppes**：询问 FIV 是否知悉并确认对其无影响
2. **gprakash**：提到客户反馈尚未到位，建议回到 w41.3 再讨论
3. **davidalb**：w38.3 -1；AR：Amy/CSE 获取客户反馈，David 移至 scoping
4. **gprakash**：明确表示"当前对 BHS 不可接受移除 GUI"；建议在 UPLR3 发布时加免责声明作为最后一次支持
5. **davidalb**（w40.3 会议纪要）：
   - Amy 确认仍有客户在用；历史可做法是"不支持但保留 + 免责声明"
   - Dell 是 BHS 代最近的 GUI 主要使用者；其他客户多已迁移
   - AR：Amy & Jackie 线下同步，下次会议提行动建议
6. **gprakash**：形成过渡方案 — 6 个月迁移窗口；UPLR3/UPLR4 并行；2026 Q3–Q4 GUI EOL

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论均来自主票据 HSD 21044505407
