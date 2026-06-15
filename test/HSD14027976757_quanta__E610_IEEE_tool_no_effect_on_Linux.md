# HSD 14027976757 总结

## 基本信息
- id: 14027976757
- title: [quantatw][E610] IPS01014913:  The IEEE tool has no effect on the E610 under Linux OS.
- customer_company: quanta
- tenant: server_platf_ae
- status: complete
- owner: otwito
- priority: p2-high
- family: Linkville Platforms
- release: Linkville 10GBASE-T Platform
- closed_reason:
- fix_description: provided more info & guidance
- submitted_date: 2026-06-03
- updated_date: 2026-06-07
- closed_date: 2026-06-07
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027976757

## 问题概述
Linux 下 IEEE 工具对 E610 不生效，现场未捕获期望信号。

## Root Cause
根因尚未完全确认，当前结论为设置/配置侧存在待进一步确认因素。

## Fix Description
provided more info & guidance

## 客户-Intel问答与分析过程
1. [2026-06-03][Customer] Q: E610在Linux下使用IEEE工具（Quartzville Tool_745290）测试：接上fixture board后示波器抓不到信号。是否有错误设置导致测试不运行/无信号输出？
   A: 可见内容中未出现Intel侧排查建议或设置校验步骤。
   分析点: 问题需要具体测试配置、驱动/NVM版本、端口/PHY模式与工具步骤，但评论仅有路由与IPS通知。
2. [2026-06-03][System/IPS] Q: （IPS通知：满足通知条件）
   A: 记录IPS编号与账号信息。
   分析点: 不包含技术问答内容。
3. [2026-06-03][System/Routing] Q: （自动路由信息）
   A: 工单被路由到对应owner/org。
   分析点: 路由信息不构成排查证据。

综合叙述：
该ticket描述了在Linux下对E610进行IEEE测试时无信号输出/示波器无波形的问题，但在可见的正文与评论中未提供Intel侧的配置检查要点、工具操作步骤、寄存器/ethtool输出或物理连接拓扑等关键证据，因此无法从现有信息判断是工具使用、驱动/NVM版本、PHY工作模式还是夹具连接导致。

证据完整性说明：
缺失证据位置：评论无任何技术回复；正文提到“Attached版本/测试设置”但当前retriever结果未包含附件内容；未提供ext_cust_blog_hist文本；缺少工具日志、ethtool/寄存器dump、示波器设置与连接示意。

## Debug步骤建议
1. 前置条件: 补齐测试环境与版本信息，确保问题可被复核。
   - 提供E610具体型号/端口形态、驱动版本、NVM版本（客户说已附但在当前文本不可见）
   - 提供Linux发行版与kernel版本、是否使用inbox驱动/自编译驱动
   - 明确测试拓扑：端口到fixture board到示波器的连接方式与测点
2. 复现: 在一致步骤下复现“无信号输出”。
   - 提供Quartzville Tool_745290的执行步骤/参数/日志输出
   - 提供示波器设置（带宽、触发、探头、终端匹配）与抓取方式
3. 定位: 区分软件未进入测试模式 vs 物理层无电/无link vs 测量方法问题。
   - 采集ethtool -i/-k/-d、link状态、speed/duplex、PHY模式信息
   - 确认测试模式下端口是否应输出特定pattern/训练信号（需工具/规格定义）
   - 检查是否存在需要的firmware/NVM配置开关或安全限制
4. 验证: 验证调整后可观察到预期信号或明确失败原因。
   - 在已知良好平台/已知良好线缆与夹具上交叉验证
   - 对比不同驱动/NVM组合是否改变输出行为
5. 收敛: 固化可复用的设置清单与判定准则。
   - 形成“工具参数-预期信号-判定方法”的最小步骤文档
   - 在票据中记录最终根因类别（配置/版本/硬件/测量）及证据日志位置

