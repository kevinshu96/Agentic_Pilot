# M3 Assumption Auditor - 最小可执行任务

## 模块目标
对解析出的假设进行证据审计，标注 Verified/Unverified/Contradicted。

## 子任务 Check List
- [ ] 定义 `AssumptionAuditorInput`、`AssumptionAuditResult`
- [ ] 实现 assumptions 为空时的处理（返回空列表，不报错）
- [ ] 实现 `contradicts()` 规则
- [ ] 实现 `supported_by()` 规则
- [ ] 实现 fallback 模式规则（默认 Unverified）
- [ ] 为每条假设生成 `justification`
- [ ] 编写单元测试：Verified/Contradicted/Unverified/fallback/empty assumptions
- [ ] 输出顺序与输入顺序一致（便于追溯）

## 完成标准
- [ ] 每条假设都有分类与理由
- [ ] 分类规则可复现，不依赖隐式状态
- [ ] fallback 情况行为稳定
