# HSD 14027968830 总结

## 基本信息
- id: 14027968830
- title: [DFI][KVL][GNR9HD] Enquiry below power pin of current consumption in S5 mode.
- customer_company: dfi
- tenant: server_platf_ae
- status: complete
- owner: chenfran
- priority: p2-high
- family: Kaseyville
- release: Kaseyville Platform GNR D Advanced / Ultra XCC
- closed_reason:
- fix_description: answered customer questions.
- submitted_date: 2026-06-03
- updated_date: 2026-06-04
- closed_date: 2026-06-04
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027968830

## 问题概述
客户咨询 S5 模式下电源引脚电流范围，属于规格/文档澄清类问题。

## Root Cause
该案例为信息咨询，无明确故障根因。

## Fix Description
answered customer questions.

## 客户-Intel问答与分析过程
1. [2026-06-03][Customer] Q: 请澄清Kaseyville Granite Rapid-D HCC SKU平台在S5模式下，VNN_NAC与VCCFA_EHV_FIVRA电源pin的电流消耗。
   A: 可见内容中未出现Intel侧给出的S5电流数值/范围/测量条件。
   分析点: 属于规格/功耗咨询，需要明确S5定义、测量点与条件，但现有文本没有后续答复。
2. [2026-06-03][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 不代表已提供规格数据。

综合叙述：
该ticket询问特定电源pin在S5下的电流消耗，但在可见的ticket正文与评论中未包含任何Intel侧数值、规格引用或测量条件说明，因此无法基于现有文本输出可用的电流消耗结论；需要补充权威规格或Intel回复记录。

证据完整性说明：
缺失证据位置：评论未包含任何电流数值或规格引用；未提供ext_cust_blog_hist；正文未包含测量条件/平台配置/rail定义映射，无法给出答案。

## Debug步骤建议
1. 前置条件: 统一S5状态定义与测量条件。
   - 明确平台S5含义（AC断电/待机电源存在/特定rail保持）与进入路径
   - 明确测量方法：板级rail电流还是pin等效电流、温度/电压条件
2. 复现: 在客户平台复现实测数据与状态。
   - 记录S5下各相关rail电压是否在规格范围
   - 记录VNN_NAC与VCCFA_EHV_FIVRA对应rail的实际电流测量值与仪表设置
3. 定位: 定位电流来源与是否符合预期。
   - 确认哪些模块在S5仍需供电（例如保持域/管理域）
   - 检查是否存在外设回灌、上拉/下拉、电源时序导致的异常电流路径
4. 验证: 验证不同条件下电流变化与合规性。
   - 对比S5与S4/S0的电流差异以确认状态正确
   - 改变输入电压/温度/负载条件（在允许范围内）观察电流趋势
5. 收敛: 输出“电流范围+条件+测量点”的可交付说明。
   - 提供范围值/典型值/最大值及其适用条件（需规格或Intel回复支撑）
   - 在票据中固化测量点定义与客户实测对比结论

