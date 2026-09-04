# State Update Policy

长期状态只能按下列链条更新，不得跳步：

`Observation → Evidence → Hypothesis → Verification → Stable State`

## 规则

1. **Observation** 只描述一次可观察行为，不写人格或能力结论。
2. **Evidence** 必须指向回答、代码、测试结果或会话日志中的具体位置。
3. **Hypothesis** 使用可证伪措辞（“可能”“待验证”），并记录 `confidence`；不得写成事实。
4. **Verification** 必须指定下一项闭卷题、陌生变式、最小实验或可复现测试。
5. **Stable State** 只有在验证完成后才能写入 `competencies.json` 的 `VERIFIED`，并满足：至少一个 `evidence`、至少一个完成的验证任务、`confidence >= 0.6`。
6. 未满足条件时只能保持 `UNKNOWN` 或 `HYPOTHESIS`，且 `verification_required` 必须为 `true`。
7. `transfer_score` 只能由未见情境的应用证据更新；一次复述或一次错误不能改变长期能力等级。
8. `constraints.md` 只接收重复出现且带 `evidence_refs` 的教学规则；单次事件写入日志或假设登记。

## 示例

```text
Observation: 用户无法解释 malloc 失败时的处理路径。
Evidence: logs/session-YYYYMMDD.md#item-<id>，回答缺少 NULL 检查与释放路径。
Hypothesis: 可能不了解动态内存生命周期（confidence=0.5）。
Verification: 下一次要求其在陌生约束下写出分配、检查、释放的最小程序。
Result: 等待验证；不得写“用户 C 语言差”。
```
