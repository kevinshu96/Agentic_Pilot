# HSD 14027961236 总结

## 基本信息
- id: 14027961236
- title: Help to Port CPLD Manager in 2700dcscm +oks
- customer_company: ami_tw
- tenant: server_platf_ae
- status: complete
- owner: hchen109
- priority: p2-high
- family: Oak Stream DMR Platforms
- release: Oak Stream Platform 1 DMR
- closed_reason: 
- fix_description: Not Intel specific design. It is Aspeed DC-SCM board related question.
- submitted_date: 2026-06-02
- updated_date: 2026-06-03
- closed_date: 2026-06-02
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027961236

## 问题概述
CPLD Manager 移植问题实质是 Aspeed DC-SCM 路由差异。

## Root Cause
I2C 总线在该板卡上经 AST1060 路由而非直连 BMC，导致 CPLD slave 不可见。

## Fix Description
Not Intel specific design. It is Aspeed DC-SCM board related question.

## 客户-Intel问答与分析过程
1. [2026-06-02][Customer] Q: 在Intel BKC BMC的CPLDM manager验证中，不同平台CPLD从设备检测不一致：OKS+2600在bus10可检测到0x51；OKS+2700DCSCM在bus10检测不到，流程相同。请协助。
   A: 可见评论中未出现Intel侧对I2C拓扑差异、bus编号映射或调试步骤的明确回复。
   分析点: 需要平台差异（硬件连接/设备树/驱动配置）与I2C扫描日志，但当前仅有问题描述。
2. [2026-06-02][System/Routing] Q: （自动路由信息）
   A: 工单被路由到对应owner/org。
   分析点: 路由不提供技术信息。

综合叙述：
该ticket描述CPLDM manager在OKS+2600与OKS+2700DCSCM平台上对CPLD slave(0x51)的检测差异，但可见文本没有Intel侧的追问与答复，也缺少I2C总线拓扑、bus编号对应关系、i2cdetect日志、驱动/设备树配置等证据，因此无法基于当前信息判断是硬件连接差异、bus号变化还是软件配置导致。

证据完整性说明：
缺失证据位置：评论无Intel侧调试建议；未提供ext_cust_blog_hist；正文缺少两平台I2C拓扑/扫描日志/错误码，无法完成根因定位。

## Debug步骤建议
1. 前置条件: 补齐两平台的I2C架构与软件栈信息。
   - 提供OKS+2600与OKS+2700DCSCM的BMC版本、CPLDM manager版本与配置
   - 提供两平台I2C拓扑/原理图要点（bus10对应的控制器与下挂器件）
2. 复现: 在两平台复现并采集一致的检测证据。
   - 提供i2cdetect/i2cdump在bus10以及相邻bus的输出
   - 记录CPLDM manager扫描日志（哪些bus、哪些地址、错误码）
3. 定位: 定位是bus编号差异、复用器选择、上电/复位时序或地址冲突。
   - 确认0x51设备是否需要先使能电源/复位/选择I2C mux通道
   - 检查bus10在2700上是否实际对应不同控制器或未连接该CPLD
   - 检查是否存在地址冲突或上拉电阻/电平转换差异导致ACK失败
4. 验证: 验证修正配置或硬件使能后可稳定检测。
   - 在2700上尝试扫描所有bus定位CPLD实际挂载bus
   - 加入必要的mux选择/延时/上电顺序后复测检测结果
5. 收敛: 输出跨平台可复用的检测策略与平台差异说明。
   - 固化“bus映射表+前置使能步骤+扫描顺序”
   - 记录最终结论：是平台拓扑差异还是软件bug，并附日志证据

