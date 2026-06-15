# M2 Analysis Parser - 最小可执行任务

## 模块目标
解析 RCA 输入（聊天文本/JSON 文件）并生成统一 `ParsedAnalysis`。

## 子任务 Check List
- [ ] 定义 `AnalysisParserInput` 与 `ParsedAnalysis`
- [ ] 实现 Mode A：聊天文本解析入口
- [ ] 实现 Mode B：JSON 文件读取与解析入口
- [ ] 定义并校验 JSON Schema（可选字段允许缺省）
- [ ] 实现 `claimedRootCause`、`mechanism`、`proposedFix` 抽取
- [ ] 实现 `evidenceCited`、`assumptions` 抽取
- [ ] 实现 `parseWarnings` 生成逻辑
- [ ] 处理文件不存在/JSON 语法错误并返回可读错误
- [ ] 编写单元测试：完整 JSON、缺字段 JSON、空 JSON、纯文本、无 root cause
- [ ] 提供解析样例（中英文混合输入）

## 完成标准
- [ ] 两种输入模式输出同一数据结构
- [ ] 解析失败有明确错误，不吞异常
- [ ] 关键缺失项会进入 warning，供评分模块使用
