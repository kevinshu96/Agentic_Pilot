# M6 Scorer - 最小可执行任务

## 模块目标
按评分矩阵输出 1-10 分整数与解释文字。

## 子任务 Check List
- [ ] 定义 `ScorerInput`、`ScorerOutput`
- [ ] 实现有 ground truth 的 base score 映射
- [ ] 实现加减分项（mechanism/fix/assumption/evidence）
- [ ] 实现 fallback 一阶原则评分逻辑
- [ ] 实现分数 clamp（1..10）
- [ ] 实现评分解释文案生成
- [ ] 覆盖规则约束：无正确类别+机制不得超过 7 分
- [ ] 编写单元测试：高分、低分、冲突假设、无证据、fallback

## 完成标准
- [ ] 同一输入输出同一分数
- [ ] 所有评分分支均有测试
- [ ] 评分解释可追溯到具体规则
