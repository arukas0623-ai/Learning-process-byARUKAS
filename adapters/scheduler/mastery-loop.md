# mastery-loop Scheduler Adapter

mastery-loop 是当前 Scheduler Adapter，负责 FSRS/SM-2、due queue、interleaving、grade、lapse、calibration 和数值统计。

接口约束：

- `due` 必须输出 Learner View，不得包含 `back`、`answer` 或 `solution`。
- 用户回答后才调用 `reveal` 获取 Evaluator View。
- Agent 不手算 interval、不手改 engine-owned `items.json`。
- `distill` 只记录蒸馏时间和摘要；语义状态仍须经过 Learner State evidence gate。
