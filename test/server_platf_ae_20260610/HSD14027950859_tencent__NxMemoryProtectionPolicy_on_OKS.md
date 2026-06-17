# HSD 14027950859 总结

## 基本信息
- id: 14027950859
- title: [TriRiver-V5] [EVT] [BIOS] Question: What is the function of NxMemoryProtection Policy on the OKS platform? Is it used on OKS?
- customer_company: tencent
- tenant: server_platf_ae
- status: complete
- owner: zhangya8
- priority: p2-high
- family: Oak Stream DMR Platforms
- release: Oak Stream Platform 1 DMR
- closed_reason:
- fix_description: provide detail infos
- submitted_date: 2026-06-01
- updated_date: 2026-06-02
- closed_date: 2026-06-02
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027950859

## 问题概述
客户询问 NxMemoryProtectionPolicy 在 OKS 平台上的功能及是否启用。

## Root Cause
此单为认知澄清类问题，根因是对策略机制和平台使用方式理解不足。

## Fix Description
provide detail infos

## 客户-Intel问答与分析过程
1. [2026-06-01][Customer] Q: OKS平台是否使用NxMemoryProtectionPolicy？NxMemoryProtectionPolicy是什么功能？
   A: 可见评论中未出现Intel侧对该policy用途与OKS是否启用的明确答复。
   分析点: 属于BIOS安全/内存保护策略解释问题，需要Intel侧定义与平台实现状态，但当前无回复。
2. [2026-06-01][System/Routing] Q: （自动路由信息）
   A: 工单被路由到对应owner/org。
   分析点: 无技术信息。

综合叙述：
该ticket询问NxMemoryProtectionPolicy在OKS平台是否使用以及其功能定义，但在可见的正文与评论中没有Intel侧的解释、引用文档或平台配置状态信息，因此无法仅基于现有文本给出该policy的用途与OKS启用情况结论。

证据完整性说明：
缺失证据位置：评论无Intel侧解释；未提供ext_cust_blog_hist；正文无任何引用文档或平台配置证据，无法回答功能定义与OKS是否使用。

## Debug步骤建议
1. 前置条件: 明确policy所在模块与可观测配置点。
   - 确认NxMemoryProtectionPolicy出现在何处（BIOS setup选项/RC策略/源码模块名）
   - 确认客户所用OKS BIOS版本与配置方式
2. 复现: 复现policy在OKS上的可见性（是否存在/是否可配置）。
   - 检查BIOS setup/配置界面是否暴露该选项，或是否可通过配置文件设置
   - 收集BIOS日志/配置dump中该policy的当前值（如可获得）
3. 定位: 定位该policy的具体作用域与影响面。
   - 在源码/文档中定位该policy控制的保护机制（例如内存页权限策略等，需证据支撑）
   - 确认其与平台功能（启动、OS兼容、安全）之间的依赖关系
4. 验证: 验证开/关该policy对系统行为的影响（在允许的安全前提下）。
   - 对比开/关时的启动日志差异、OS启动稳定性与相关保护特性状态
   - 确保任何变更符合平台安全要求与回归测试
5. 收敛: 给出对外说明与平台结论。
   - 输出：功能定义、OKS是否使用/默认值、何时需要开启/关闭的条件
   - 记录证据来源（文档章节/日志片段/配置dump）以便复查

