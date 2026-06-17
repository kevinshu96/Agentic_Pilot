# HSD 14027409449 总结

## 基本信息
- id: 14027409449
- title: Hash check corrupts architectural registers (R14/R15) across the LT SHA routine
- tenant: ipu_process.feature
- status: complete
- owner: suneilmo
- priority: 2-high
- family: Birch Stream Platform
- release: Birch Stream Platform SRF AP
- scope_type: bug_fix
- fw_sw_ingredient: starcode
- customers_requesting: (空)
- submitted_date: 2026-03-16
- hsd_url: https://hsdes.intel.com/appstore/article-one/#/article/14027409449

## Problem Overview
在 **OS late patchload（WRMSR 0x79）** 场景下，从微码 `m_01_a06f3_830003a3` 升级到 `m_01_a06f3_830003b1` 时，OS 触发 **#GP（General Protection Fault）**，进而引发 **kernel panic**（"Timeout during microcode update"）。

日志中观察到 R14/R15 被写成 **non-canonical 地址值**（如 `R14: 26809950ce7bd6c7`），导致执行路径使用非法地址并触发 #GP。

## Root Cause
新增的 **hash check / LT SHA 例程**在执行过程中使用了 **R14 与 R15** 作为工作寄存器，但 ucode patch/patchload 流程**未正确 save/restore R14/R15** 的架构状态，导致 patchload 结束后这两个寄存器值被污染，OS 路径中使用污染值引发 #GP。

## Fix Description
在 ucode patch/patchload 流程中补齐对 **R14/R15 的正确 save/restore**，确保 hash check 例程不会将工作寄存器状态泄露到调用者环境。

验证：使用修复后测试 patch（`m_01_a06f3_831003b1`）确认 late microcode reload 正常完成，版本从 `0x830003a3` 更新到 `0x831003b1`，不再触发 #GP / kernel panic。

## Comments 关键讨论（按时间顺序）
1. **suneilmo**：从 kernel panic log 判断 #GP 由 non-canonical address 引起；R14 含有异常地址值，怀疑与寄存器被破坏有关
2. **suneilmo**：Mauricio 在 Atom lab 复现；CPU hangdump 显示 patch 已加载完成；#GP 发生在 WRMSR 0x79 完成后，指向 patchload flow 中 R14 被破坏
3. **suneilmo**：Randall 定位到新增 hash check / LT SHA 例程使用 R14/R15 但未 save/restore；测试 patch `m_01_a06f3_831003b1` 验证 patchload 正常
4. **suneilmo**：结论 — 这是 **ucode patch bug**；patch flow 必须跨 patchload 正确保存/恢复 R14/R15
5. **davidalb**：GK Decision — **GK Approved for UPLR4 SRF AP/SP**（线下批准）
6. **kamanna**：评估该问题发生在 UPLR3→preUPLR4 期间，UPLR4 发布前已修复，客户不太会命中，倾向 go silent
7. **kamanna**：Sid 确认 — **go silent**（不对外宣讲/文档化）

## Link HSD 信息
- No Info（当前接口未返回可用的 Link HSD 字段）

## 信息补充说明
- 本文结论均来自主票据 HSD 14027409449（含 description 与 comments）

## 关联日志下载信息
- 日志下载状态: 未发现可提取的附件/日志链接
- 日志文件命名规则: HSD#14027409449_<原文件名>
- 日志文件列表: No Info
- 备注: 当前HSD详情中未提取到可下载日志链接；如后续出现附件链接可补下载。


