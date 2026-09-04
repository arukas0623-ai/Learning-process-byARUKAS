# Learner State Update Policy

所有长期状态修改必须遵循：

`Observation → Evidence → Hypothesis → Verification → Stable State`

- Observation 只描述一次可观察行为。
- Evidence 必须指向回答、代码、测试或日志中的具体位置。
- Hypothesis 必须可证伪，并带 `confidence`；不得伪装成事实或人格判断。
- Verification 必须指定下一项闭卷题、陌生变式、实验或复测。
- Stable State 只能由验证结果支持；单次错误不得生成“用户不会 X”。
- 状态写入后运行实现提供的 validator；失败则拒绝升级。
