# HSD 14027969662 总结

## 基本信息
- id: 14027969662
- title: Iperf Performance in 2P (6984P-C) mainboard with 2 CX7 (200Gbps) for bidirection
- customer_company: tencent
- tenant: server_platf_ae
- status: complete
- owner: qliang3
- priority: p2-high
- family: Birch Stream Platform
- release: Birch Stream Platform GNR AP
- closed_reason: 
- fix_description: customer question clarified
- submitted_date: 2026-06-03
- updated_date: 2026-06-08
- closed_date: 2026-06-08
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027969662

## 问题概述
2P 双 CX7 双向 iperf 吞吐问题通过绑定策略优化后达到预期。

## Root Cause
初始性能受限于 core/NUMA 绑定不当。

## Fix Description
customer question clarified

## 客户-Intel问答与分析过程
1. [2026-06-03][Customer] Q: 2P(6984P-C)主板配两张CX7(200G)且每CPU驱动一张CX7；两卡互跑iperf测交叉吞吐，双向两端口200G时CPU是否会成为性能瓶颈？
   A: 可见内容中未出现Intel侧对CPU瓶颈/NUMA/PCIe带宽/测试方法的明确答复。
   分析点: 属于性能咨询类问题，需系统拓扑/PCIe链路/NUMA绑核/协议栈参数等，但现有文本缺失。
2. [2026-06-04][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 没有技术结论与验证数据。

综合叙述：
该ticket提出2P平台上双CX7在双向200Gbps压测时CPU是否限制性能的咨询，但正文与评论中未提供任何Intel侧分析结论，也缺少平台PCIe拓扑、NUMA绑定、iperf参数、CPU利用率与中断分布等关键证据，因此无法仅据现有内容判断瓶颈归因。

证据完整性说明：
缺失证据位置：评论无技术答复；未提供ext_cust_blog_hist；缺少iperf参数、CPU/IRQ/NUMA统计、PCIe链路信息与实际吞吐数据。

## Debug步骤建议
1. 前置条件: 确认硬件拓扑与测试目标定义。
   - 确认每张CX7的PCIe代际/链路宽度、插槽挂载到哪个CPU root complex
   - 确认测试模式：单流/多流、TCP/UDP、单端口/双端口、单方向/双向
2. 复现: 按统一方法复现性能数据并记录资源消耗。
   - 记录iperf命令行参数（并发数、窗口、绑定IP/CPU）
   - 记录吞吐、丢包/重传、CPU利用率（usr/sys/softirq）
3. 定位: 区分CPU瓶颈、PCIe瓶颈、网络栈/驱动瓶颈或测试方法限制。
   - 检查每端口的实际PCIe带宽利用与链路状态
   - 检查IRQ亲和性与NUMA绑定（应用绑核/内存绑节点）
   - 检查是否启用/配置RSS、RPS/XPS、GRO/LRO等
4. 验证: 通过调参或隔离实验验证瓶颈点。
   - 调整并发流数量与绑核策略观察吞吐是否线性增长
   - 单向与双向对比，单端口与双端口对比，跨NUMA与同NUMA对比
5. 收敛: 输出可执行的性能上限判断与推荐配置。
   - 给出瓶颈归因结论（CPU/PCIe/驱动/配置）及支持数据
   - 沉淀推荐：绑核/IRQ/RSS队列/iperf并发的最小配置清单

