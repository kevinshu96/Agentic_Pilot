# HSD 13015497875 总结

## 基本信息
- id: 13015497875
- title: Integer overflow that can trigger OOB
- tenant: ipu_process.feature
- status: rejected
- owner: bhonigst
- priority: 2-high
- family: Intel Platform Update (IPU)
- release: IPU 2027.1
- scope_type: passenger
- fw_sw_ingredient: CSME
- customers_requesting: (空)
- submitted_date: 2026-05-20
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/13015497875

## Problem Overview
在 `bup_ese_proxy.c` 的 `bup_psk_blob_read()` 函数中，`blob_id` 和 `dbg` 来自 ESE 外部消息。代码计算 `entry_id = blob_id * 2 + dbg`，但 `entry_id` 为 `uint8_t`。当 `blob_id=128, dbg=0` 时：
- 计算结果 256 超过 uint8_t 范围，溢出回绕为 0
- 边界检查（基于 `SPDM_PSK_BLOBS_NUM=8`）被绕过
- 后续按溢出前逻辑偏移访问可产生 **OOB read**（示例偏移 `256 × 80 = 20,480`）

`bup_psk_blob_write()` 存在类似风险，描述中认为不会导致 OOB write 但建议也加校验。

## Root Cause
- `entry_id` 使用窄类型 `uint8_t` 承载乘加计算结果，可发生整型溢出回绕
- 对来自外部消息的 `blob_id`/`dbg` 缺少充分的范围与合法性校验，边界检查基于溢出后值，实际访问偏移仍可越界

## Fix Description
No Info（ticket 状态 rejected，filed_by_mistake，未提供已落地修复）

建议方向（来自描述）：
- 使用更宽的中间类型（如 `uint16_t/uint32_t`）计算后再赋值
- 在赋值前显式校验 `entry_id` 合法范围
- 对 `blob_id`/`dbg` 输入做严格边界校验
- write 路径同步补齐检查

## Comments 关键讨论
- 该票 comments 字段为空，无可用评论信息

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论均来自主票据 HSD 13015497875
- 证据不足：comments 为空，无额外讨论可提取

## 关联日志下载信息
- 日志下载状态: 已发现附件链接并尝试下载，下载失败
- 日志文件命名规则: HSD#13015497875_<原文件名>
- 已发现的附件/日志链接:
  - https://hsdes.intel.com/rest/binary/13014624463
  - https://hsdes.intel.com/rest/binary/13014624464
  - https://hsdes.intel.com/rest/binary/13014624465
  - https://hsdes.intel.com/rest/binary/13014624466
  - https://hsdes.intel.com/rest/binary/13014624467
  - https://hsdes.intel.com/rest/binary/13014624468
  - https://hsdes.intel.com/rest/binary/13014624469
  - https://hsdes.intel.com/rest/binary/13014624552
- 下载失败原因: 401 Unauthorized（当前终端会话未携带可用HSD认证凭据）
- 备注: 获得HSD下载认证后可重试下载并按命名规则落盘。


