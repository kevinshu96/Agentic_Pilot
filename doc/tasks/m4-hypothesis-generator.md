# M4 Hypothesis Generator - 最小可执行任务

## 模块目标
在已有 RCA 之外生成至少 2 条可检验的替代根因假设。

## 子任务 Check List
- [ ] 定义 `HypothesisGeneratorInput`、`HypothesisGeneratorOutput`
- [ ] 实现 symptom 推断逻辑（基于 claimed root cause + reasoning）
- [ ] 建立候选假设池（硬件/固件/软件/配置/时序/环境）
- [ ] 实现已排除假设识别（ruled_out 抽取）
- [ ] 实现候选过滤与去重
- [ ] 确保最少返回 2 条替代假设
- [ ] 每条假设附带简短可验证理由
- [ ] 编写单元测试：标准输入、空根因、大量已排除项

## 完成标准
- [ ] 固定输入下输出稳定（可预测）
- [ ] 不返回已被明确排除的假设
- [ ] 始终满足“至少 2 条”约束
