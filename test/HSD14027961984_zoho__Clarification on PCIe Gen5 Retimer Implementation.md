# HSD 14027961984 总结

## 基本信息
- id: 14027961984
- title: Clarification on PCIe Gen5 Retimer Implementation
- customer_company: zoho
- tenant: server_platf_ae
- status: complete
- owner: veeraval
- priority: p2-high
- family: Kaseyville
- release: Kaseyville Platform GNR D Advanced / Ultra XCC
- closed_reason: 
- fix_description: Answered customer queries
- submitted_date: 2026-06-02
- updated_date: 2026-06-05
- closed_date: 2026-06-05
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027961984

## 问题概述
客户咨询移除 Gen5 retimer 的影响，已给出插损与设计指导。

## Root Cause
该单为设计取舍咨询，无明确故障根因。

## Fix Description
Answered customer queries

## 客户-Intel问答与分析过程
1. [2026-06-02][Customer] Q: Boulder Mesa Fab C设计中使用Phison PCIe Gen5 Retimer（PS7202-74，U108423/U108424）：其目的/用例是什么？能否移除并将PCIe lane直连MCIO？移除后对性能、SI、PCIe compliance和功能有何影响？
   A: 可见内容中未出现Intel侧对是否可移除及影响评估的具体答复。
   分析点: 需要基于链路预算、走线长度/损耗、拓扑与合规要求给出结论，但当前仅有提问。
2. [2026-06-05][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 缺少任何技术评审结论或条件化建议。

综合叙述：
该ticket询问PCIe Gen5 retimer在参考设计中的用途以及是否可去除并直连MCIO的影响，但在现有可见的正文与评论中没有Intel侧的SI/合规/性能评估结论、链路长度与损耗数据或替代方案说明，因此无法仅基于当前文本判断是否可移除及其风险边界。

证据完整性说明：
缺失证据位置：评论无任何技术答复；未提供ext_cust_blog_hist；正文未包含链路长度/插损/拓扑细节与任何SI/合规分析数据。

## Debug步骤建议
1. 前置条件: 收集做retimer取舍所需的链路与板级参数。
   - 提供从CPU/Root到MCIO的走线长度、层叠、材料、插损预算与连接器数量
   - 明确目标速率（PCIe Gen5 x?）与合规目标（Base spec/平台要求）
2. 复现: 在现有设计上建立基线（带retimer）性能与稳定性。
   - 记录链路训练结果、速率降级情况、AER错误统计
   - 进行带负载的压力测试（吞吐、稳定性）
3. 定位: 评估是否存在必须retimer的SI/合规原因。
   - 基于插损/回损/串扰预算评估直连是否满足Gen5眼图/抖动余量
   - 检查参考设计中retimer放置点是否用于跨板/长走线/连接器链路补偿
4. 验证: 验证去除retimer后的可行性（理论+实验）。
   - 仿真：对直连拓扑进行SI仿真并对比余量
   - 实验：如可行，制作去除retimer的验证板或配置跳线，并做训练/误码/AER对比
5. 收敛: 给出条件化结论与设计指导。
   - 输出“可移除/不可移除/需满足条件”的结论及阈值（长度/插损/连接器数）
   - 说明对性能（是否降速）、稳定性（错误率）、合规（测试项）与功能（热插拔/恢复等）的影响

