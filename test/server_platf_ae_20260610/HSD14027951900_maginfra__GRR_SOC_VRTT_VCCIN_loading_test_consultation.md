# HSD 14027951900 总结

## 基本信息
- id: 14027951900
- title: [Maginfra][Zijin DPU4.0] GRR SOC VRTT VCCIN loading test consultation
- customer_company: maginfra
- tenant: server_platf_ae
- status: complete
- owner: lijian4
- priority: p2-high
- family: Loganville Platforms
- release: Loganville-BTS-HCC
- closed_reason:
- fix_description: Question is answered. Close this IPS ticket.
- submitted_date: 2026-06-01
- updated_date: 2026-06-03
- closed_date: 2026-06-03
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027951900

## 问题概述
客户在 PVCCIN 负载测试中观察到功耗超过边界，需确认测试约束与可行范围。

## Root Cause
VRTT/VR 的热约束限制了可承载电流与功耗上限（约 90A/160W）。

## Fix Description
Question is answered. Close this IPS ticket.

## 客户-Intel问答与分析过程
1. [2026-06-01][Customer] Q: 使用GRR PI测试治具BGA2147-GRR-Red Interposer测试：PVCCIN最大可拉载功耗160W，但按Iccmax=110A、VID=1.8V计算功耗约179W超过限制；VRTT也无法拉载到该电流。是否可按97A(160W折算极限)作为最大测试值，保持transient step与slew rate不变，仅将ICCMAX改为99A？
   A: 可见内容中未出现Intel侧对是否允许降低Iccmax/如何调整测试方法的明确答复。
   分析点: 问题涉及测试方法合规性与治具能力限制，需要Intel侧确认边界与替代验证方式，但当前没有回复。
2. [2026-06-02][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 缺少最终建议或批准结论。

综合叙述：
该ticket围绕GRR PI红板治具功耗/电流上限（160W约97A）与CPU Iccmax目标（110A）不匹配的问题，客户提出是否可降低测试最大电流并调整ICCMAX参数以在治具能力内完成测试。但现有可见文本未包含Intel侧的认可/替代方案/验证覆盖建议，因此无法基于当前内容判断该调整是否满足测试意图与规范要求。

证据完整性说明：
缺失证据位置：评论无Intel侧答复与批准结论；未提供ext_cust_blog_hist；缺少测试实测曲线/治具规格条款/最终采用的ICCMAX与验证结果。

## Debug步骤建议
1. 前置条件: 明确测试目标与治具能力边界。
   - 确认CPU目标Iccmax、VID/Vboot条件与测试规范要求的覆盖点
   - 确认interposer的160W限制对PVCCIN与VRTT分别适用的条件与测量方式
2. 复现: 复现治具在高VID下无法达到目标电流的现象。
   - 记录在VID=1.7/1.8V时可达到的最大电流与功耗曲线
   - 记录VRTT静态/动态拉载失败的具体表现与限制点
3. 定位: 定位限制来自治具功耗上限、电源回路能力还是配置参数。
   - 确认治具限制是否为热/电流保护触发，或供电回路饱和
   - 核对计算公式与实际测量功耗是否一致（含电压降项）
4. 验证: 验证降低最大电流与ICCMAX设置后，关键瞬态指标是否仍满足测试意图。
   - 在97A/99A设置下保持transient step与slew rate，测量关键响应指标并对照规范
   - 评估是否需要额外补充测试来覆盖110A角落（若治具无法覆盖）
5. 收敛: 给出可执行的测试裁剪/替代验证方案与风险说明。
   - 明确是否接受以治具极限为上限的测试结论，以及需声明的限制
   - 如需覆盖110A：给出替代手段（更高能力治具/分段验证/仿真等）并记录证据要求

