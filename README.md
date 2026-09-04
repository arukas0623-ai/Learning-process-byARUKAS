# AgentLearn

AgentLearn 是一个可迁移的 Personal Learning Intelligence Framework（个人学习智能框架）。

它给 Agent 提供一套稳定的认知接口，使不同 Agent 能够用较少上下文恢复学习工作：读取当前状态，区分事实与推测，根据证据决定下一步，并把重要结果写回长期状态。

AgentLearn 不是聊天记录归档器、固定课程表、题库、数据库或 UI 应用。

## 解决的问题

普通的学习对话容易出现几个问题：

- 新 Agent 不知道用户目前学到哪里。
- Agent 把“听过”“能解释”“能独立完成”混为一谈。
- 一次答对或答错被误判为长期能力。
- 计划脱离真实证据，变成固定课程表。
- 用户的学习历史和个人目标被误提交到公开仓库。

AgentLearn 用以下边界解决这些问题：

1. 能力判断必须有证据。
2. 证据不足时保留 \`UNKNOWN\`。
3. “知道、能解释、能应用、能迁移”分别判断。
4. 学习路线根据状态动态生成，不预设固定课表。
5. 公共协议与个人状态严格分离。
6. Agent 交接使用固定、可读取的结构。

## 核心概念

### AgentLearn Identity

AgentLearn 的定位是：

> 一个帮助 Agent 建模学习状态、验证能力、追踪成长并生成下一步行动的框架。

它不替宿主 Agent 提供 system instruction，也不替换宿主已有的 skill、command 或 framework。AgentLearn 只提供自己的 namespace 能力和文件协议。

### Ability Map

Ability Map 描述“一个领域需要哪些能力”，例如编程基础、数据结构或算法思想。它是可替换的领域配置，不绑定教材、课程或某一种知识源。

### Learner State

Learner State 描述“当前有哪些能力已经被证据支持”。它不保存未经验证的印象，也不把聊天原文直接当作能力结论。

推荐的能力状态包括：

- \`UNKNOWN\`：没有足够证据。
- \`HYPOTHESIS\`：有初步观察，但还没有完成验证。
- \`VERIFIED\`：已有可复核证据，并通过验证。
- \`NEEDS_PRACTICE\`：概念可能理解，但应用或迁移仍不稳定。

具体状态枚举以项目中的 schema 和实例协议为准。

### Evidence Gate

长期状态只能沿着以下证据链更新：

\`\`\`text
Observation → Evidence → Hypothesis → Verification → Stable State
\`\`\`

含义如下：

- \`Observation\`：记录实际观察到的行为，例如回答、代码、测试结果或 Debug 过程。
- \`Evidence\`：保存可以复核的证据摘要和来源，不保存整段聊天原文。
- \`Hypothesis\`：根据证据提出暂时判断。
- \`Verification\`：通过新问题、独立实现、反例、测试或迁移任务验证判断。
- \`Stable State\`：只有证据足够且验证通过，才允许形成稳定能力状态。

一次正确或错误回答通常只能产生观察，不能直接改变长期能力。

## 公共与私有数据边界

公共仓库只保存通用框架，不保存任何特定用户的状态。

推荐目录结构：

\`\`\`text
.agentlearn/                         # 公共：协议、schema、模板
├── skill.md
├── protocols/
│   ├── boot.md
│   ├── handoff.md
│   └── capability-interface.md
├── schemas/
│   ├── learner-state.schema.json
│   ├── evidence.schema.json
│   └── decision.schema.json
└── templates/
    ├── learner-state.template.json
    └── identity.template.md

.agentlearn-private/                  # 私有：本地使用，必须被 Git 忽略
├── identity.md
├── learner_state.json
├── active_goals.json
├── knowledge_debt.json
├── evidence/
├── decisions/
└── history/
\`\`\`

\`.agentlearn-private/\` 可以换成其他明确被忽略的私有路径，但不能把真实个人数据放入公共 \`.agentlearn/\`、README、公开 example 或 Git history。

私有目录可以保存：

- 学习者自定义偏好。
- 当前能力状态和目标。
- 知识债务。
- 证据索引、验证结果和学习决策。
- 历史摘要。

私有目录不需要保存完整聊天记录。应优先保存短摘要、事实、证据来源、置信度和时间。

## 低 Token 启动协议

新 Agent 接管时，先读取最小必要上下文，顺序固定如下：

\`\`\`text
1. .agentlearn/skill.md
2. .agentlearn/protocols/boot.md
3. .agentlearn-private/identity.md       # 存在时读取
4. .agentlearn-private/learner_state.json
5. .agentlearn-private/active_goals.json
6. .agentlearn-private/knowledge_debt.json
\`\`\`

启动阶段不要默认读取：

- 全部历史。
- 全部日志。
- 全部 evidence 文件。
- 全部 decisions 文件。
- 完整题库、完整能力地图或全部代码。

只有当前任务需要时，才按索引读取 \`evidence/\`、\`decisions/\`、\`history/\` 或具体知识源。

文件不存在时：

- 不创建虚假状态。
- 对文件本身报告 \`NOT_AVAILABLE\`。
- 对无法判断的能力报告 \`UNKNOWN\`。

启动读取只负责恢复上下文，不自动开始教学，不自动出第一道题，也不自动修改学习状态。

启动摘要建议只包含：

\`\`\`text
Current Stage
Active Goals
Knowledge Debt
Recent Progress（已有摘要时）
Next Evidence Action
Uncertainty
\`\`\`

## IRLA 学习循环

每次学习活动使用：

\`\`\`text
Input → Recall → Learning Validation → Application
\`\`\`

### Input

提供一小段材料、一个概念、一道问题或一个真实任务。Input 必须和当前能力状态及目标相关。

### Recall

要求学习者闭卷回忆、解释、追踪或提出思路。

Recall 阶段只展示 Learner View，例如题目内容和必要上下文；在学习者回答前，不展示答案、解法、评分标准或 Evaluator View。

### Learning Validation

学习者回答后，Agent 才能检查：

- 概念是否正确。
- 执行过程是否正确。
- 是否能说明原因。
- 是否存在边界遗漏或误解。

验证结果应记录为证据摘要，而不是直接给出夸大的长期结论。

### Application

要求学习者把理解用于代码、伪代码、调试、建模或实际问题。必要时增加 Transfer：使用陌生输入、反例或不同表述检查迁移能力。

学习结束后，按项目实例要求执行：

\`\`\`text
记录证据 → 运行 state validator → distill 摘要
→ 更新下一次复习 → 输出简短结果
\`\`\`

Scheduler 负责 FSRS、interval、lapse 和复习时间；Agent 负责提问、解释、追问、评分和证据记录。两者职责不能混淆。

## 动态路线规则

Agent 不生成脱离状态的固定课程表，而是为当前阶段选择最小的下一步证据任务：

| 当前证据状态 | 下一步行动 |
|---|---|
| 基础能力为 \`UNKNOWN\` | 先做短诊断，建立可验证观察 |
| 有观察但仍是 \`HYPOTHESIS\` | 设计验证题、反例或独立实现 |
| 概念已 \`VERIFIED\` | 进入应用，检查代码和实际解题 |
| 概念会做但不稳定 | 生成针对性练习，检查重复错误 |
| 能在陌生问题中迁移 | 提高问题复杂度或减少提示 |
| 出现重复误解 | 针对误解生成训练，不扩大结论范围 |

每个下一步行动应说明：

- 要验证什么能力。
- 需要学习者提交什么证据。
- 什么结果算通过或未通过。
- 完成后更新哪个状态。

## AgentLearn Capability Interface

这些是 AgentLearn 的 namespace 能力，不是通用 slash command。宿主可以用自己的调用方式映射它们，不应创建或要求 \`/status\`、\`/plan\`、\`/review\` 这类可能冲突的命令。

### \`inspect_state\`

用途：读取并压缩当前状态。

输入：可选的领域、目标或状态路径。

输出：

- 当前阶段。
- 当前目标。
- 主要知识债务。
- 最近变化（有摘要时）。
- 已知事实、假设和未知项。
- 下一步最小证据行动。

### \`generate_plan\`

用途：根据证据生成下一阶段行动，不生成固定课表。

必须考虑：

- 当前能力状态。
- 已有 evidence。
- 知识债务和重复误解。
- 应用/迁移结果。
- 时间限制。
- 学习目标。

输出应包含行动、成功标准、失败后的分支以及状态写回方式。

### \`learning_review\`

用途：分析最近一次或一组学习活动。

可输入：commit、notes、tasks、phase reports、assessment 或日志索引。

输出：

- 已观察到的能力。
- 尚未解决的问题。
- 可能的误解。
- 下一步建议。
- 对应证据和置信度。

只保存摘要和证据引用，不复制聊天全文。

### \`handoff_package\`

用途：为另一个 Agent 生成最小可用的接管上下文。

输出必须使用以下固定顺序：

\`\`\`text
AgentLearn Handoff Package

Facts
Assumptions
Unknowns
Evidence
Active Decisions
Next Actions
\`\`\`

各部分含义：

- \`Facts\`：文件或证据直接支持的事实。
- \`Assumptions\`：为了继续工作而暂时采用的推测，不能当成事实。
- \`Unknowns\`：目前没有足够证据判断的内容。
- \`Evidence\`：支持事实或判断的摘要及来源。
- \`Active Decisions\`：当前仍有效的设计决定和理由。
- \`Next Actions\`：下一 Agent 应优先执行的有限步骤。

接管 Agent 应先读取该包，再按需打开具体证据；不能因为某项写在 \`Assumptions\` 中就把它升级为稳定能力。

## 证据来源

状态判断可以引用以下真实来源：

- Git history 和 commit message。
- phase report。
- 学习 notes。
- assessment 结果。
- 任务产出和测试结果。
- 具体知识源中的可复核内容。

证据层的目标不是收集所有资料，而是让 Agent 能回答：“我为什么这样判断？”

一个能力声明至少应能关联：

\`\`\`json
{
  "claim": "能力或状态声明",
  "evidence": ["可复核证据摘要或引用"],
  "confidence": 0.0,
  "timestamp": "YYYY-MM-DD"
}
\`\`\`

置信度不能替代验证。即使置信度较高，没有足够证据时仍应保持 \`UNKNOWN\` 或 \`HYPOTHESIS\`。

## 可替换的适配边界

AgentLearn 的核心协议不绑定具体工具：

\`\`\`text
Knowledge Source  →  Ability Map  →  Learner State
       可替换           可替换           私有实例

Agent  ↔  AgentLearn Protocol  ↔  Scheduler Adapter
\`\`\`

- \`Knowledge Source\`：教材、文档、课程、题目或用户提供的材料。
- \`Ability Map\`：某个领域的能力分解。
- \`Scheduler\`：复习时间、间隔和遗忘调度。
- \`AgentLearn Protocol\`：状态、证据、评估和交接规则。

替换知识源不应改变 Learner State 的证据规则；替换 Ability Map 不应把个人数据带入公共框架；替换 Scheduler 不应让 Agent 越权修改复习算法。

## 快速接入

### 对维护者

1. 将通用协议放入 \`.agentlearn/\`。
2. 将真实学习者数据放入 \`.agentlearn-private/\` 或其他 ignored 路径。
3. 从 \`.agentlearn/templates/\` 复制模板，按需要创建私有状态文件。
4. 配置领域 Ability Map、Knowledge Source 和 Scheduler。
5. 让宿主 Agent 读取 \`skill.md\` 和 \`protocols/boot.md\`。
6. 首次启动只恢复状态，不自动开始教学。

### 对接管 Agent

可使用如下最小提示：

\`\`\`text
加载 AgentLearn。
读取 .agentlearn/skill.md 和 .agentlearn/protocols/boot.md。
按低 Token 顺序读取存在的 .agentlearn-private 状态文件。
不要读取全部历史，不要猜测缺失状态。
先输出 Facts、Unknowns、当前目标、知识债务和 Next Evidence Action，暂不开始教学。
\`\`\`

开始学习后，使用 IRLA；学习结束后，按 Evidence Gate 更新状态，并在有 Scheduler 时更新下一次复习。

### Codex 中的调用方式

如果宿主 Codex 已接入 AgentLearn Skill，可使用其 namespace 能力：

\`\`\`text
$agentlearn inspect_state
$agentlearn generate_plan
$agentlearn learning_review
$agentlearn handoff_package
\`\`\`

这只是 Codex 的映射示例，不是 AgentLearn 对所有宿主强制要求的命令格式。

## 隐私与提交检查

提交公共仓库前，必须确认：

- 没有真实姓名、个人目标、性格描述或能力评价。
- 没有真实 learner state、evidence、history、decision 或聊天记录。
- \`.agentlearn-private/\` 已被 \`.gitignore\` 忽略。
- 公开 example 只包含通用模板或去标识化示例。
- Git 暂存区没有私有文件。

建议执行：

\`\`\`bash
git status --ignored
git diff --cached
git grep -i "姓名\|个人目标\|性格\|学习历史" -- .
\`\`\`

最后一条命令的关键词应按项目实际情况补充；扫描结果必须人工确认，不能只依赖命令退出码。

## 仓库内容

- \`.agentlearn/\`：公共 Skill、协议、schema 和模板。
- \`examples/\`：不包含真实个人数据的领域实例或示例。
- \`core/\`、\`adapters/\`、\`agents/\`：框架实现和接入协议（如果该版本包含）。
- \`.agentlearn-private/\`：本地个人实例，不属于公共仓库。

更详细的协议定义请从以下入口开始阅读：

- \`.agentlearn/skill.md\`
- \`.agentlearn/protocols/boot.md\`
- \`.agentlearn/protocols/handoff.md\`
- \`.agentlearn/protocols/capability-interface.md\`
- \`.agentlearn/schemas/\`
- \`.agentlearn/templates/\`

## 当前范围

本项目专注于：

- 学习状态建模。
- 证据门槛和能力验证。
- IRLA 学习循环。
- 动态下一步行动。
- 低 Token 上下文恢复。
- 跨 Agent 交接。
- 公共协议与个人数据隔离。

本项目当前不要求：

- 复杂 UI。
- 数据库。
- 云端同步。
- 新的 AI Provider。
- 通用 Agent 编排框架。
- 绑定某一本教材或某一种课程体系。

## License

本项目采用 MIT License，除非具体文件另有说明。
