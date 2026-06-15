# M7 Report Formatter - 最小可执行任务

## 模块目标
按照固定模板渲染最终报告，保证条件段落按规则显示/隐藏。

## 子任务 Check List
- [ ] 定义 `ReportFormatterInput`
- [ ] 固化报告模板章节顺序
- [ ] 实现 `HSD Summary` 条件渲染（有 HSD 才显示）
- [ ] 实现 `Comparison to Ground Truth` 条件渲染（非 fallback 才显示）
- [ ] 实现 Assumption/Aternative/Score/Next Steps 章节渲染
- [ ] 空列表场景渲染占位文本（如 none extracted）
- [ ] 编写快照测试：完整场景、无 HSD、fallback、空审计项
- [ ] 保障输出为纯 Markdown（不含非法格式）

## 完成标准
- [ ] 输出章节稳定，不随输入波动乱序
- [ ] 条件渲染符合详细设计
- [ ] 结果可直接发送给用户
