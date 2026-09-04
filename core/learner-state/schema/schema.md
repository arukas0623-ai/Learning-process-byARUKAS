# Learner State Schema

Learner State 是用户资产，不属于某个 Agent。实现可使用 Markdown + JSON；字段名保持稳定，内容由用户控制。机器可读约束见 `learner-state.schema.json`。

最小对象：

```json
{
  "id": "stable-id",
  "status": "UNKNOWN|HYPOTHESIS|VERIFIED",
  "recall_score": null,
  "explanation_score": null,
  "application_score": null,
  "transfer_score": null,
  "observed_facts": [],
  "model_hypotheses": [],
  "verifications": [],
  "evidence": [],
  "confidence": 0.0,
  "verification_required": true
}
```

`UNKNOWN` 和 `null` 表示没有足够证据，不表示零分。`VERIFIED` 的必要条件由 [evidence rules](../evidence-rules/rules.md) 定义。
