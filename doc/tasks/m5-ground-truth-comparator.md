# M5 Ground Truth Comparator - 最小可执行任务

## 模块目标
将 `ParsedAnalysis` 与 HSD ground truth 对比，输出匹配级别、一致点和分歧点。

## 子任务 Check List
- [ ] 定义 `ComparatorInput`、`ComparisonResult`
- [ ] 实现 fallback 直返逻辑（`skipped=true`）
- [ ] 实现 `rootCauseMatch` 评估（exact/category/partial/wrong/unknown）
- [ ] 实现 `mechanismMatch` 评估（correct/partial/wrong/unknown）
- [ ] 实现 `fixMatch` 评估（aligned/partial/misaligned/unknown）
- [ ] 实现 agreements 收集
- [ ] 实现 divergences 收集
- [ ] 编写单元测试：exact/category/wrong/fallback

## 完成标准
- [ ] 三类 match 字段均可解释、可复现
- [ ] fallback 不做误判比较
- [ ] 输出可直接供 M6 评分
