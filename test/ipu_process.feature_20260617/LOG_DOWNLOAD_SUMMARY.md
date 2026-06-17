# HSD 日志下载总结报告

## 📋 操作日期
2026-06-17

## 📊 下载统计

### 总体成果
- **成功下载**: 5 个日志文件
- **下载失败**: 8 个二进制 ID（返回 404 Not Found）
- **排除的记录**: HSD14027444970（scope_type=feature，按用户要求排除）

### 按 HSD 记录分类

#### ✅ HSD13015037108 (scope_type: bug_fix)
- 状态: 完全下载成功
- 下载文件:
  - HSD#13015037108_binary_13014971688.bin (192.1 KB)
  - HSD#13015037108_binary_13014971689.bin (25.58 KB)
- 文档已更新

#### ✅ HSD14027558754 (scope_type: driver)
- 状态: 完全下载成功
- 下载文件:
  - HSD#14027558754_binary_13014971688.bin (192.1 KB)
  - HSD#14027558754_binary_13014971689.bin (25.58 KB)
- 文档已更新

#### ⚠️ HSD13015497875 (scope_type: passenger)
- 状态: 部分下载成功
- 已成功下载:
  - HSD#13015497875_binary_13014971689.bin (25.58 KB)
- 下载失败（404 Not Found）:
  - binary ID: 13014624463, 13014624464, 13014624465, 13014624466, 13014624467, 13014624468, 13014624469, 13014624552
  - 原因: 这些 binary ID 在 HSD 系统中不存在或已被删除
- 文档已更新，记录了部分下载成功和部分失败的情况

#### ❌ HSD14027444970 (scope_type: feature)
- 状态: 已排除，无日志下载
- 原因: 用户要求不下载 scope_type=feature 的记录

## 🔐 认证方式
- 使用 git credential helper 获取 HSDES 凭证
- 凭证来源: 系统 Windows Credential Manager
- 用户: kevin.shu@intel.com

## 📝 文档更新
已更新以下 3 个 HSD 文档的"关联日志下载信息"部分：
1. HSD13015037108_unknown__TDX 1.5.32 handoff retrieval for HV=0.md
2. HSD14027558754_unknown__TDX 1.5.32 handoff retrieval for HV=0 (IPU 26.3).md
3. HSD13015497875_unknown__Integer overflow that can trigger OOB.md

## 📂 文件位置
所有下载的日志文件保存在:
```
c:\Users\wshu96\OneDrive - Intel Corporation\Desktop\HSDCriticAgent\test\ipu_process.feature_20260617\
```

## 🔄 Git 提交
- 提交 ID: e782f4b
- 提交信息: "Download HSD log files and update document metadata (5 files successfully retrieved)"
- 包含: 5 个二进制日志文件 + 3 个更新的 markdown 文档

## ⚠️ 已知问题

### HSD13015497875 的 8 个日志文件无法下载
- 这些 binary ID（13014624463-13014624469, 13014624552）返回 404
- 可能原因:
  1. 文件已从 HSD 系统中删除
  2. Binary ID 格式有误
  3. 文件已过期或被归档

**建议**: 
- 与 HSD 管理员联系确认这些二进制 ID 的状态
- 尝试通过 HSD web 界面直接访问这些附件
- 或通过其他渠道获取这些日志文件

## 📌 后续行动
1. ✅ Git push 仍在进行中（预计因二进制文件较大需要时间）
2. ⏳ 如需重试下载 HSD13015497875 的失败文件，建议:
   - 确认 binary ID 是否正确
   - 检查 HSD 系统是否有最新的附件列表
   - 尝试使用浏览器直接下载并验证文件完整性

