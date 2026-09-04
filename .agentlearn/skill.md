# AgentLearn Skill

## AgentLearn Identity

AgentLearn 是一个 **Personal Learning Intelligence Framework**。

它帮助支持 Skill、Instruction 或 Context Loading 的 Agent：

1. 建模学习状态
2. 基于证据判断能力
3. 追踪成长过程
4. 生成下一步行动
5. 保持长期连续性

AgentLearn 是认知协议，不是通用 Agent 框架、UI、数据库或 AI Provider。

## Agent Operating Rules

- 优先读取私有状态，再读取公共协议和按需证据。
- 不假设学习者掌握程度；缺少证据时使用 `UNKNOWN`。
- 区分知道、能解释、能应用和能迁移。
- 每个能力判断必须有 claim、evidence、confidence 和 timestamp。
- 单次正确或错误行为不能直接升级或降级长期能力。
- 优先推动可验证能力增长，而不是堆积信息。
- 不保存聊天原文，只保存可复核摘要。
- 不覆盖宿主 Agent 的 system instruction、skill、command 或 framework 配置。

## Context Loading

低 token 启动顺序：

1. `.agentlearn/skill.md`
2. `.agentlearn-private/identity.md`（如果存在）
3. `.agentlearn-private/learner_state.json`
4. `.agentlearn-private/active_goals.json`
5. `.agentlearn-private/knowledge_debt.json`

只有在当前任务需要追溯时，才读取：

- `.agentlearn-private/evidence/`
- `.agentlearn-private/decisions/`
- `.agentlearn-private/history/`

文件不存在时输出 `NOT_AVAILABLE` 或 `UNKNOWN`，不得创建虚假状态；不得默认递归读取完整历史、日志、代码或题库。

## Evidence Gate

长期状态变更遵循：

`Observation → Evidence → Hypothesis → Verification → Stable State`

只有 Evidence 可定位、Verification 完成且 validator 通过时，才允许写入 `VERIFIED`。目标、计划或学习材料本身不能证明能力。

## AgentLearn Capabilities

以下是 `AgentLearn` namespace 能力描述，不是通用 slash command：

- `inspect_state`：读取当前阶段、目标、风险和最近变化。
- `generate_plan`：根据状态、知识债务、时间限制和目标生成最小可验证下一步，不生成固定课程表。
- `learning_review`：基于 commit、notes、tasks、logs 等证据摘要分析已获得能力、未解决问题和下一步。
- `handoff_package`：生成 Facts、Assumptions、Unknowns、Evidence、Active Decisions、Next Actions 交接包。

详细输入输出见 `.agentlearn/protocols/capability-interface.md`。

## Public / Private Boundary

公共仓库只保存本 Skill、通用协议、Schema、Template、Capability 描述和 Framework 文档。个人身份、目标、能力判断、证据、历史和项目状态只能位于明确 ignored 的 `.agentlearn-private/`。
