# AgentLearn Handoff Protocol

本协议是通用交接指令，不包含任何具体学习者信息；个人状态只从本地 `.agentlearn-private/` 读取。

## 接管顺序

先读取 `skill.md`，再按 `boot.md` 读取 identity、learner state、active goals 和 knowledge debt；最近进展、决策和证据按需加载。

## Handoff Package

输出固定结构：

```text
AgentLearn Handoff Package

Facts
Assumptions
Unknowns
Evidence
Active Decisions
Next Actions
```

## 内容规则

- **Facts**：只写有来源的可验证事实。
- **Assumptions**：写成可证伪假设，附 confidence 和 falsifier。
- **Unknowns**：明确缺失证据，不把未知改写成不会。
- **Evidence**：给出 source_ref、时间和支持的 claim。
- **Active Decisions**：说明仍有效的决策、理由和来源。
- **Next Actions**：给出最小可验证行动、成功证据、失败分支和回写位置。

不得输出聊天原文，不得把一次行为写成稳定能力，不得默认读取完整历史或全部代码。
