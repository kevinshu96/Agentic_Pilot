# M1 HSD Fetcher - 最小可执行任务

## 模块目标
从 HSD 系统获取工单信息，并输出标准化 ground truth 结构；在不可用场景进入 fallback。

## 子任务 Check List
- [ ] 定义输入输出结构：`HSDFetcherInput`、`HSDFetcherOutput`
- [ ] 实现 HSD 编号规范化（支持 `HSD#12345678`、`12345678`）
- [ ] 封装 `get_hsdes_issue_info` 调用
- [ ] 封装 `get_hsdes_issue_summary` 调用
- [ ] 实现状态映射（closed/open/not_found）
- [ ] 实现 root cause、fix summary、key evidence 提取函数
- [ ] 实现 `fallbackMode` 判定逻辑
- [ ] 实现错误返回结构（网络失败、未找到、字段缺失）
- [ ] 编写单元测试：closed/open/not_found/no_hsd/closed_without_root_cause
- [ ] 提供模块 README 示例输入输出（便于下游联调）

## 完成标准
- [ ] 对外返回结构稳定，字段齐全
- [ ] 所有核心分支有测试覆盖
- [ ] fallback 场景可被 M5/M6 正确消费
