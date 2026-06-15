# HSD 14027951869 总结

## 基本信息
- id: 14027951869
- title: [Changkuai ] PCI SERR error occurs when the E810 network adapter operates in Legacy mode
- customer_company: kuaishou
- tenant: server_platf_ae
- status: complete
- owner: feiwang
- priority: p2-high
- family: system.Columbia Park
- release: system.Columbia Park.Columbiaville_SD
- closed_reason:
- fix_description: NA
- submitted_date: 2026-06-01
- updated_date: 2026-06-05
- closed_date: 2026-06-05
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027951869

## 问题概述
Legacy 模式 PXE 期间出现 PCI SERR 错误，关联 ACS violation。

## Root Cause
E810 legacy driver 在 PXE 路径存在 ACS 规则不兼容行为，触发 CPU root port ACS violation。

## Fix Description
NA（当前可行规避是关闭 ACS，暂无永久修复）。

## 客户-Intel问答与分析过程
1. [2026-06-01][Customer] Q: AMD 7Y83服务器搭配E810-XXVDA2 OCP NIC，在启用Legacy mode进行PXE boot时触发PCI SERR错误。请分析根因。
   A: 可见内容中未出现Intel侧对SERR触发路径、BIOS/Option ROM交互或规避方案的答复。
   分析点: 需要SERR日志、AER/PCI配置空间、PXE流程与BIOS设置，但当前仅有问题描述。
2. [2026-06-05][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 无技术结论或复现/修复信息。

综合叙述：
该ticket报告E810在Legacy PXE场景下触发PCI SERR错误，但可见的正文与评论中没有Intel侧进一步问答、错误日志、复现步骤细节或修复建议，因此无法仅凭现有文本判断是平台BIOS、Legacy Option ROM、PCIe错误报告设置还是NIC固件/配置导致。

证据完整性说明：
缺失证据位置：评论无Intel侧分析与建议；未提供ext_cust_blog_hist；正文缺少SERR日志/AER信息/详细PXE复现步骤与BIOS配置截图或文本。

## Debug步骤建议
1. 前置条件: 固化平台与启动配置，确保SERR可复现且可采证。
   - 确认BIOS版本、Legacy/UEFI设置、PXE ROM启用状态与启动顺序
   - 确认E810固件版本（正文提到4.30）与端口配置
2. 复现: 复现PXE期间的SERR并采集日志。
   - 获取平台事件日志/串口/BIOS调试输出中关于SERR的记录
   - 如可用，采集PCIe AER日志与错误计数（OS侧或预OS工具）
3. 定位: 定位SERR来源设备与触发时刻（Option ROM执行、链路训练、DMA等）。
   - 确认SERR是由NIC Function还是上游bridge报告（需错误源信息）
   - 对比Legacy PXE与UEFI PXE/OS启动是否同样触发，以缩小到Legacy ROM路径
4. 验证: 验证规避/修复措施是否消除SERR。
   - 尝试更新/切换NIC固件或PXE ROM版本并复测
   - 调整BIOS中PCIe错误报告相关选项（如有）并复测（需记录具体改动）
5. 收敛: 输出根因、修复版本与可操作workaround。
   - 在票据中固化：复现步骤、日志片段、对比实验结果
   - 给出最终建议（BIOS设置/固件版本/禁用Legacy等）及影响范围

