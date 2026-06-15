# HSD 14027950994 总结

## 基本信息
- id: 14027950994
- title: [REL]When BIOS uses 5-level paging, Linux sometimes fails to boot  because the BIOS detects  a page-fault.
- customer_company: fujitsu_japan
- tenant: server_platf_ae
- status: complete
- owner: lning
- priority: p2-high
- family: Birch Stream Platform
- release: Birch Stream Platform GNR SP
- closed_reason:
- fix_description: shared how to build thier required EDK files.
- submitted_date: 2026-06-01
- updated_date: 2026-06-02
- closed_date: 2026-06-02
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027950994

## 问题概述
5-level paging 场景下 Linux 偶发启动失败并报 page fault。

## Root Cause
UniversalPayload 的 setpagetablepoolreadonly() 在该场景下错误设置只读属性，导致 #PF。

## Fix Description
更新 UniversalPayload 二进制（含 commit 0f3867fa6ef0553e26c42f7d71ff6bdb98429742 所对应修正）并指导客户构建所需 EDK 文件。

## 客户-Intel问答与分析过程
1. [2026-06-01][Customer] Q: GNR-SP的Intel Reference Code请求：请更新UniversalPayload.elf（DEBUG/RELEASE）以包含commit 0f3867...，解决5-level paging + ≥1TB内存场景下Linux偶发因BIOS #PF而无法启动的问题；根因是SetPageTablePoolReadOnly()不支持5-level paging导致错误标记只读区域。
   A: Intel侧在评论中解释：Birch Stream在CWF Alpha/Beta期间未批准更新到2024 Nov稳定标签，当前验证基于edk2-stable202402；该commit在edk2-stable202411才包含。建议客户/BIOS团队自行cherry-pick该commit并重建UniversalPayload.elf，提供了具体命令与构建说明链接。
   分析点: 问答闭环较完整：明确为何未集成（稳定标签策略/回归风险），并给出可执行的自助构建与回迁建议。
2. [2026-06-01][Intel (comment by kwatanab)] Q: 为何该commit未应用？若应应用，请请求BIOS团队集成。
   A: 触发进一步解释与替代路径（回迁/自建）。
   分析点: 典型的“确认缺失原因→确定可行集成路径”的互动。
3. [2026-06-01][Intel (comment by ndhaller)] Q: 是否需要更新EDK2 stable tag？如何在现有基线下解决？
   A: 说明不升级到edk2-stable202411，可能仅允许把该commit backport到内部override分支；同时强调Fujitsu不应被阻塞，可自行实现公开fix；提供FDBin ReadMe与两种构建方式（-pb None或UniversalPayloadBuild.py）。
   分析点: 给出策略边界与操作步骤，属于可执行的debug/修复建议。
4. [2026-06-02][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 从对话看客户认为已有足够信息可自行处理。

综合叙述：
该ticket围绕5-level paging与大内存场景下Linux因BIOS #PF偶发启动失败的问题展开，客户明确给出根因与所需EDK2修复commit。Intel侧解释该修复未进入当前Birch Stream验证基线的原因（仍基于edk2-stable202402，commit在edk2-stable202411），并提供了可操作的解决路径：在客户/BIOS团队侧对EDK2 cherry-pick该commit后重建UniversalPayload.elf（给出具体命令与参考链接），从而在不升级整套stable tag的情况下修复问题。

证据完整性说明：
可见证据主要来自评论区ndhaller/kwatanab的详细说明与命令行步骤。缺失项：ext_cust_blog_hist未提供；未包含客户实际#PF日志与修复后验证记录（是否已在客户侧实测通过）。

## Debug步骤建议
1. 前置条件: 确认问题触发条件与代码基线。
   - 确认BIOS启用5-level paging且内存规模约1TB或更高
   - 确认当前UniversalPayload.elf构建所基于的EDK2版本为edk2-stable202402（或等价SHA）
   - 确认Linux侧启用KASLR且可将kernel放置到高地址区域
2. 复现: 复现#PF导致Linux启动失败并采集证据。
   - 采集BIOS检测到#PF的日志/串口信息与触发地址范围
   - 记录一次失败中kernel实际装载地址与页表只读保护区域的关系（若可获得）
3. 定位: 确认SetPageTablePoolReadOnly()在5-level paging下的保护区域计算错误。
   - 核对当前代码是否缺少commit 0f3867fa6ef0553e26c42f7d71ff6bdb98429742
   - 对照示例：页表位于某地址且size约34MB时，是否出现错误保护大范围高地址区域的现象
4. 验证: 应用修复并验证问题消失。
   - 在EDK2中cherry-pick该commit并重建UniversalPayload.elf（DEBUG/RELEASE）
   - 在相同条件下进行多次冷启动/重启，确认不再出现#PF启动失败
5. 收敛: 固化交付物与版本/流程说明。
   - 记录最终采用的EDK2基线SHA + cherry-pick commit信息 + 生成的elf校验信息（如有）
   - 在票据中归档构建命令与参考链接，明确不升级stable tag、仅回迁单点修复的策略

