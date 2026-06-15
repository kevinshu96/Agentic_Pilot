# HSD 14027986827 总结

## 基本信息
- id: 14027986827
- title: 【E810】E810_driver_W11_v1.21.113.0 lack of 4ID under [Intel.NTamd64.10.0.1..26100] section which will cause upgrade driver from (09/04/2025,1.19.36.0) to 1.21.113.0  driver  failure under windows 26100(Win11 24H2) or later
- customer_company: foxconn
- tenant: server_platf_ae
- status: complete
- owner: feiwang
- priority: p2-high
- family: system.Columbia Park
- release: system.Columbia Park.Columbiaville_LD
- closed_reason:
- fix_description: add the customer's 4-part pci id into the inf file
- submitted_date: 2026-06-04
- updated_date: 2026-06-05
- closed_date: 2026-06-05
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027986827

## 问题概述
Windows 11 24H2 下驱动升级失败，定位到 INF 配置缺失。

## Root Cause
INF 文件未包含目标 4-part PCI ID（pci\\ven_8086&dev_159b&subsys_40038086），导致升级匹配失败。

## Fix Description
add the customer's 4-part pci id into the inf file

## 客户-Intel问答与分析过程
1. [2026-06-04][Customer] Q: E810_driver_W11_v1.21.113.0 的 icea.inf 在 [Intel.NTamd64.10.0.1..26100] 段缺少4ID（含 PCI\VEN_8086&DEV_159B&SUBSYS_40038086），导致Win11 24H2(26100)或更高版本从1.19.36.0升级到1.21.113.0失败并提示已安装最佳驱动。
   A: 可见内容中未出现对“缺少ID是否为包缺陷/是否会修复/修复方式”的明确回复。
   分析点: 正文提供了较清晰的根因假设（INF缺ID）与复现步骤，但缺少Intel侧确认与修复计划证据。
2. [2026-06-04][Intel (comment by cjsmith)] Q: 这是 follow-on 工单，客户标记为Critical。
   A: 确认了严重性与关联背景，但未提供技术结论。
   分析点: 属于项目管理信息，不等同于技术答复。
3. [2026-06-05][Intel (comment by glin8)] Q: 当前组件build状态？
   A: 当前component build已完成，等待RE容器build。
   分析点: 说明内部构建流程进行中，但未明确修复内容是否包含补齐INF缺失ID。
4. [2026-06-05][System/IPS] Q: （客户在IPS侧关闭）
   A: 记录为Customer Closed in IPS。
   分析点: 关闭不代表问题已技术解决；缺少“已修复版本/补丁/验证结果”。

综合叙述：
该ticket给出了明确的现象与推测根因：在Win11 24H2(26100)段的INF缺少设备ID，导致驱动升级被系统判定不匹配而失败。评论区仅体现工单的严重性关注与内部构建状态更新，但未看到对缺失ID的确认、修复提交、发布版本号或回归验证记录，因此无法仅凭当前文本确认最终解决方案与结论。

证据完整性说明：
缺失证据位置：评论未包含对INF缺失ID的确认或修复说明；未提供ext_cust_blog_hist内容；缺少修复后版本/补丁链接/安装日志与回归结果。

## Debug步骤建议
1. 前置条件: 固定复现环境与对比基线。
   - 确认OS版本为Windows 11 24H2 (Build 26100或更高)
   - 确认旧驱动版本1.19.36.0与目标1.21.113.0包来源一致（WHQL/内部包路径）
   - 确认目标设备硬件ID（含SUBSYS）与客户描述一致
2. 复现: 复现升级失败与“best driver already installed”提示。
   - 在设备管理器/PNPUtil执行升级并记录日志（安装返回码、setupapi.dev.log关键信息）
   - 对比升级前后驱动版本是否保持在1.19.36.0
3. 定位: 确认是否为INF匹配缺陷以及缺失的具体条目。
   - 检查 icea.inf 中 [Intel.NTamd64.10.0.1..26100] 是否包含对应PCI\VEN_8086&DEV_159B&SUBSYS_40038086
   - 对比其它OS段（例如不同NT版本段）是否包含该ID以验证“仅26100段缺失”的假设
   - 确认签名/目录文件与INF修改的一致性要求（修改INF后需重新签名）
4. 验证: 验证修复后驱动可升级安装。
   - 使用补齐ID并正确签名的驱动包进行升级验证
   - 验证升级后设备驱动版本变更为1.21.113.0并功能/链路正常
5. 收敛: 形成可交付结论与发布信息。
   - 给出修复包版本号/发布日期/变更点（补齐哪些ID、覆盖哪些OS段）
   - 记录验证矩阵（OS build、设备ID、升级路径）并在票据评论固化

