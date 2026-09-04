# AgentLearn Capability Interface

这些能力属于 `AgentLearn` namespace，不创建 `/status`、`/plan`、`/review` 等宿主通用命令。

## `inspect_state`

输入：可选 `scope`、`goal_id`、`competency_ids`。

读取：Boot Protocol 的最小状态文件，按需追踪 Evidence 引用。

输出：Current Stage、Active Goals、Major Risks、Recent Changes、Evidence、Uncertainty。

## `generate_plan`

输入：当前能力状态、Evidence、Knowledge Debt、misconception、时间限制和目标。

输出：一个或多个最小可验证行动：`action`、`why_now`、`prerequisites`、`success_evidence`、`failure_route`、`writeback_target`。

规则：`UNKNOWN` 先诊断，`HYPOTHESIS` 先验证，`VERIFIED` 才进入应用和陌生迁移；不得生成固定课程表。

## `learning_review`

输入：commit、notes、tasks、logs 的路径或摘要，不要求聊天原文。

输出：Observed Capability、Unresolved Problems、Misconceptions、Next Action、Evidence References、Confidence。

## `handoff_package`

输入：Skill、Boot Protocol、最小状态和必要证据。

输出：

```text
AgentLearn Handoff Package
Facts
Assumptions
Unknowns
Evidence
Active Decisions
Next Actions
```

输出前必须检查：个人信息未进入公共文档；每个重要判断有 source_ref；未知和假设没有被写成事实。
