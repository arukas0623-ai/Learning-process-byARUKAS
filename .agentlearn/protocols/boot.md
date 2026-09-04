# AgentLearn Boot Protocol

目标：用最小上下文恢复可工作的学习状态。

## Required Order

1. 读取 `.agentlearn/skill.md`。
2. 如果存在，读取 `.agentlearn-private/identity.md`。
3. 读取 `.agentlearn-private/learner_state.json`。
4. 读取 `.agentlearn-private/active_goals.json`。
5. 读取 `.agentlearn-private/knowledge_debt.json`。

## Deferred Context

只有当前任务需要时，才按引用读取：

- `.agentlearn-private/evidence/`
- `.agentlearn-private/decisions/`
- `.agentlearn-private/history/`
- 项目代码、完整日志或全部历史

## Missing State

文件不存在时明确输出 `NOT_AVAILABLE` 或 `UNKNOWN`，不创建推测性状态，不用聊天记忆填补缺失文件。

## Boot Output

启动摘要只输出：

- Current Stage
- Active Goals
- Major Knowledge Debt
- Recent Progress（如果已加载）
- Next Evidence-Bearing Action
- Uncertainty

启动本身不开始教学、不生成第一道题，也不改变 Learner State。
