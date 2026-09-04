# AgentLearn

AgentLearn 是一个可迁移的个人学习智能框架。

它解决两件事：

1. 让用户可以用简单的话告诉 Agent“我想学什么”，或上传自己的学习材料。
2. 让不同 Agent 能快速恢复学习状态，并根据真实证据决定下一步。

AgentLearn 不是课程平台、题库、聊天记录仓库、数据库或 UI 应用。它是一套由 Skill、协议、状态文件和可替换适配器组成的学习接口。

## 先看这里

### 如果你只是想开始学习

你不需要先理解目录、JSON 或 Git。直接把下面这段话复制给支持 AgentLearn 的 Agent，再把方括号内容改成自己的信息：

```text
我想学习：［填写主题，例如 C 语言、英语、数学或软件工程］
我的目标是：［填写目标，例如能独立写程序、通过考试或完成项目］
我目前的基础是：［不知道也可以写“不确定”］

请使用 AgentLearn。
先读取当前学习状态；如果没有状态，就明确说明 UNKNOWN。
不要直接给我长课程表。
按照 Input → Recall → Learning Validation → Application 工作。
每次只给我一个当前最合适的问题或任务。
在我回答前，不要展示答案、完整解法或评分标准。
学习结束后，只根据真实证据更新状态。
```

如果你还没有学习状态，Agent 应先做短诊断，而不是假设你已经掌握了某些知识。

### 如果你有学习材料

你可以把 PDF、Markdown、TXT、代码、图片或课程笔记直接上传到 Agent 对话中，然后发送：

```text
请把我刚上传的材料作为本次学习的 Knowledge Source。
先告诉我材料覆盖哪些主题，以及它们可能对应哪些能力。
不要直接开始长篇讲解。
先根据我的当前状态选择一个最小的 Input，并进入 Recall。
不要在我回答前显示答案、完整解法或评分标准。
```

如果材料要长期放在本地，可以放在私有目录：

```text
.agentlearn-private/sources/
```

然后告诉 Agent：

```text
请读取 .agentlearn-private/sources/［文件名］，
把它作为本次学习的 Knowledge Source。
只读取完成当前任务所需的文件，不要递归读取全部历史。
```

带有姓名、账号、学习记录、私人笔记或其他个人信息的材料，不要放进公开仓库，也不要提交到 GitHub。

## 用户操作指南

### 第一步：安装或取得项目

AgentLearn 没有单独的桌面安装程序。最简单的方式是下载仓库，或者使用 Git 克隆：

```bash
git clone https://github.com/arukas0623-ai/Learning-process-byARUKAS.git
cd Learning-process-byARUKAS
```

Windows 用户也可以：

1. 打开仓库页面。
2. 点击绿色的 `Code` 按钮。
3. 选择 `Download ZIP`。
4. 解压文件。
5. 用支持 Skill 或项目文件读取的 Agent 打开解压后的项目目录。

如果你已经在 Codex 中打开了这个项目，不需要再次安装仓库；直接使用“告诉 Agent 学习什么”的方式即可。

### 第二步：初始化个人学习空间

个人状态必须放在私有目录，不放在公共框架目录。

推荐目录：

```text
.agentlearn-private/
├── identity.md
├── learner_state.json
├── active_goals.json
├── knowledge_debt.json
├── sources/
├── evidence/
├── decisions/
└── history/
```

最简单的方法是让 Agent 初始化：

```text
请在当前项目初始化 AgentLearn 私有学习空间：
1. 读取公共 .agentlearn/ 协议和模板。
2. 创建 .agentlearn-private/ 及必要的状态文件。
3. 把私有目录加入或确认已加入 .gitignore。
4. 不要读取、复制或提交不必要的个人信息。
5. 初始化完成后只汇报文件是否可用，不要开始教学。
```

也可以手动复制模板：

```powershell
New-Item -ItemType Directory -Force .agentlearn-private | Out-Null
New-Item -ItemType Directory -Force .agentlearn-private/sources | Out-Null
New-Item -ItemType Directory -Force .agentlearn-private/evidence | Out-Null
New-Item -ItemType Directory -Force .agentlearn-private/decisions | Out-Null
New-Item -ItemType Directory -Force .agentlearn-private/history | Out-Null
Copy-Item .agentlearn/templates/identity.template.md .agentlearn-private/identity.md
Copy-Item .agentlearn/templates/learner-state.template.json .agentlearn-private/learner_state.json
```

如果 Agent 尚未创建 active_goals.json 或 knowledge_debt.json，让 Agent 根据公共协议创建最小有效文件；不要自行猜测文件内容。

### 第三步：告诉 Agent 你想学什么

最简单的说法就是：

```text
我想学习［主题］。
目标是［目标］。
请先读取 AgentLearn 状态，然后开始一次短诊断。
```

更完整的说法：

```text
我想学习［主题］，目标是［具体结果］。
我每次大约有［时间］。
我目前知道［已知内容；不知道就写不确定］。
我希望你［例如：每次只问一个问题、用中文、少讲解、多让我练习］。

请使用 AgentLearn 的 IRLA 流程。
先做必要的 Input 和 Recall，不要直接生成固定课程表。
```

你不需要知道 Ability Map 的格式，也不需要手动填写能力评分。Agent 应从你的回答、代码、任务结果和测试中收集证据。

### 第四步：开始一次学习活动

一次学习活动通常这样进行：

```text
Input
Agent 提供一个小概念、材料片段、问题或任务。

Recall
你闭卷回答、解释、追踪或写出思路。
此阶段只看问题，不看答案。

Learning Validation
你回答后，Agent 检查概念、过程、边界和原因。

Application
你把知识用于代码、伪代码、练习、调试或真实任务。
必要时再做 Transfer，检查陌生问题中的迁移能力。
```

你可以直接这样开始：

```text
开始今天的 AgentLearn 学习。
先按低 Token 启动流程读取状态。
不要展示状态文件内部细节。
只给我第一个问题，不要给答案。
```

如果你暂时不想开始，只想查看状态：

```text
请执行 AgentLearn inspect_state。
只汇报当前阶段、目标、知识债务、最近变化和不确定项。
不要开始教学。
```

### 第五步：结束学习并保存结果

学习结束时可以说：

```text
结束本次学习。
请根据本次真实表现记录 Observation 和 Evidence，
运行 state validator，必要时更新 Hypothesis 或 Stable State，
执行 distill，并由 Scheduler 更新下一次复习时间。
只输出简短摘要，不保存聊天全文。
```

一次答对或答错不应直接变成长期能力结论。没有足够证据时，状态必须保持 `UNKNOWN` 或 `HYPOTHESIS`。

## 个人材料如何安全地上传

有三种方式，按方便程度选择：

### 方式 A：直接上传到 Agent 对话

适合临时学习：

1. 在 Agent 对话中上传文件。
2. 说明“把这个文件作为本次 Knowledge Source”。
3. 指定你想学习的章节、主题或问题。
4. 要求 Agent 先提取范围，再开始 IRLA。

示例：

```text
我上传了《［文件名］》。
我想学习其中的［章节或主题］。
请先提取本次需要的最小内容，再按 IRLA 提问。
不要把文件全文复制进长期状态。
```

### 方式 B：放进本地私有 sources

适合重复使用：

```text
.agentlearn-private/sources/
```

只在提示中指定需要的文件：

```text
使用 .agentlearn-private/sources/intro.md。
只读取与“［主题］”有关的部分。
把它作为 Knowledge Source，不要把原文写入 learner state。
```

### 方式 C：发布去标识化的公共材料

只有在材料已经删除个人信息，并且你确实希望公开时，才放入公共 examples/ 或其他公开目录。

公开前检查：

```text
删除姓名、账号、个人目标、个人学习历史、私人笔记和本地绝对路径。
把材料改成通用示例。
确认它不依赖任何私有状态。
```

公开的领域示例可以描述某个学科需要哪些能力，但不能包含某个真实学习者的能力判断或学习过程。

## 公共目录与私有目录

公共仓库保存“AgentLearn 怎么工作”；私有目录保存“某个学习者现在是什么状态”。

```text
.agentlearn/                         # 公共，可提交 GitHub
├── skill.md                         # AgentLearn 身份与总规则
├── protocols/
│   ├── boot.md                      # 低 Token 启动
│   ├── handoff.md                   # Agent 交接格式
│   └── capability-interface.md      # 能力接口
├── schemas/
│   ├── learner-state.schema.json
│   ├── evidence.schema.json
│   └── decision.schema.json
└── templates/
    ├── learner-state.template.json
    └── identity.template.md

.agentlearn-private/                  # 私有，本地使用，不提交
├── identity.md
├── learner_state.json
├── active_goals.json
├── knowledge_debt.json
├── sources/                          # 用户自己的学习材料
├── evidence/                         # 证据摘要
├── decisions/                        # 有效设计或学习决策
└── history/                          # 历史摘要
```

必须遵守：

- 公共目录不保存真实用户状态。
- 私有目录不应被 Git 跟踪。
- 不把私有材料、答案、聊天全文或历史记录复制到 README。
- 缺失的状态不能由 Agent 编造。
- 公开 example 必须是通用或去标识化内容。

## 给 Agent 的最小接管说明

以下部分是给 Agent 读取的。新 Agent 不需要先阅读整个仓库；按下面顺序读取即可：

```text
AgentLearn = Personal Learning Intelligence Framework。

启动顺序：
1. 读取 .agentlearn/skill.md。
2. 读取 .agentlearn/protocols/boot.md。
3. 读取存在的 .agentlearn-private/identity.md。
4. 读取 learner_state.json、active_goals.json、knowledge_debt.json。
5. 只在当前任务需要时读取 evidence、decisions、history 或 Knowledge Source。

规则：
- 缺失文件报告 NOT_AVAILABLE；无法判断的能力保持 UNKNOWN。
- 不递归读取全部历史、日志、代码或题库。
- 不把一次行为升级为长期能力。
- 区分知道、能解释、能应用和能迁移。
- 学习使用 Input → Recall → Learning Validation → Application。
- 状态使用 Observation → Evidence → Hypothesis → Verification → Stable State。
- Recall 阶段只展示 Learner View；回答前不展示答案、解法或评分标准。
- Scheduler 负责 FSRS、interval、lapse 和复习时间。
- Agent 负责提问、解释、追问、验证、评分和证据记录。
- 永远不读取、引用、提交或上传私有状态到公共仓库。
```

更完整的规则位于：

```text
.agentlearn/skill.md
.agentlearn/protocols/boot.md
.agentlearn/protocols/handoff.md
.agentlearn/protocols/capability-interface.md
```

启动阶段只输出恢复上下文，不自动开始第一道题，除非用户明确要求开始学习。

## AgentLearn 能力接口

这些是 AgentLearn namespace 能力，不是通用 slash command。宿主 Agent 可以用自己的方式调用，不要创建 /status、/plan、/review 等容易冲突的命令。

### inspect_state

读取并压缩当前状态，输出：

- 当前阶段。
- 当前目标。
- 主要知识债务。
- 最近变化（存在摘要时）。
- Facts、Assumptions、Unknowns。
- 下一步最小证据行动。

### generate_plan

根据当前能力、证据、知识债务、误解、迁移结果、时间限制和目标，生成最小下一步行动。

它不是固定课程表。每个行动应写清：

- 要验证的能力。
- 学习者要提交的证据。
- 成功标准。
- 失败后的下一步。
- 需要写回的状态位置。

### learning_review

根据 commit、notes、tasks、assessment、phase report 或日志索引分析最近学习行为，输出：

- 已观察到的能力。
- 尚未解决的问题。
- 可能的误解。
- 下一步建议。
- 证据引用和置信度。

只保存摘要和来源引用，不保存聊天全文。

### handoff_package

为下一个 Agent 输出最小接管上下文，固定使用：

```text
AgentLearn Handoff Package

Facts
Assumptions
Unknowns
Evidence
Active Decisions
Next Actions
```

新 Agent 先读这个包，再按需读取具体证据。假设不能当成事实，未知不能被补写成已知。

## 状态与证据规则

能力状态遵循以下证据链：

```text
Observation → Evidence → Hypothesis → Verification → Stable State
```

一个状态声明应能回答三个问题：

1. Agent 实际观察到了什么？
2. 哪些证据可以复核？
3. 为什么当前判断足够稳定，或为什么仍然未知？

证据摘要至少应包含：

```json
{
  "claim": "能力或状态声明",
  "evidence": ["可复核证据摘要或 source_ref"],
  "confidence": 0.0,
  "timestamp": "YYYY-MM-DD"
}
```

confidence 不能代替验证。没有足够证据时，即使 Agent 感觉很确定，也必须保持 UNKNOWN 或 HYPOTHESIS。

## 动态学习路线

Agent 应根据证据选择下一步：

| 证据情况 | 行动 |
|---|---|
| 能力为 UNKNOWN | 做短诊断，建立观察 |
| 有观察但仍是 HYPOTHESIS | 用反例、验证题或独立实现检查 |
| 已通过验证 | 进入 Application |
| 应用不稳定 | 针对错误生成小练习 |
| 能处理陌生问题 | 增加难度或减少提示 |
| 重复出现同一误解 | 生成针对误解的训练 |

路线必须是动态的，不绑定某一本教材，也不提前生成与状态无关的长期课程表。

## Agent 交接

接管 Agent 只需要恢复“当前仍然有效的最小事实”。

交接包必须区分：

- Facts：来源直接支持的事实。
- Assumptions：暂时采用的推测。
- Unknowns：没有足够证据的内容。
- Evidence：事实或判断的来源摘要。
- Active Decisions：仍有效的决定及理由。
- Next Actions：下一 Agent 需要做的有限动作。

推荐的交接提示：

```text
请接管这个 AgentLearn 学习任务。
先读取 .agentlearn/skill.md、boot.md 和当前私有状态。
如果存在 Handoff Package，先按 Facts、Assumptions、Unknowns、Evidence、Active Decisions、Next Actions 恢复。
不要读取全部历史，不要猜测缺失状态，不要开始教学。
先输出最小接管摘要，等待我的下一步指令。
```

## Codex 中的使用方式

如果 Codex 已接入 AgentLearn Skill，可以使用以下 namespace 调用：

```text
$agentlearn inspect_state
$agentlearn generate_plan
$agentlearn learning_review
$agentlearn handoff_package
```

自然语言也可以：

```text
请执行 AgentLearn inspect_state。
请生成下一步最小证据行动，不要生成固定课程表。
请根据本次学习生成 handoff package。
```

这些不是 AgentLearn 强制规定的通用 slash command。其他 Agent 可以使用等价的自然语言或工具接口。

## 提交 GitHub 前的隐私检查

公共仓库可以包含通用协议、模板、schema 和去标识化的领域 example，但不能包含个人学习状态。

提交前执行：

```bash
git status --ignored
git diff --cached
git ls-files .agentlearn-private
```

确认：

- git ls-files .agentlearn-private 没有输出。
- 暂存区没有私有文件。
- 没有真实姓名、账号、个人目标、性格分析或能力评价。
- 没有 learner state、evidence、history、decision 或聊天全文。
- 没有本地绝对路径或私人学习材料。

如果要检查常见个人信息关键词：

```bash
git grep -n -i -E "姓名|账号|个人目标|性格|学习历史|私人笔记" -- .
```

命令命中 README 中的隐私规则文字并不代表泄露；需要人工检查命中的上下文。

## 适配器边界

以下部分都可以替换：

```text
Knowledge Source  →  Ability Map  →  Learner State
       可替换           可替换           私有实例

Agent  ↔  AgentLearn Protocol  ↔  Scheduler
```

- Knowledge Source：教材、文档、课程、题目、代码或用户上传材料。
- Ability Map：某个领域需要掌握的能力结构。
- Learner State：当前学习者状态，只写入有证据支持的内容。
- Scheduler：复习时间、间隔和遗忘调度。
- AgentLearn Protocol：启动、评估、状态和交接规则。

替换 Knowledge Source 不应改变证据门槛；替换 Ability Map 不应暴露个人数据；替换 Scheduler 不应改变 Agent 的评估职责。

## 适合扩展的内容

可以添加：

- 新领域的通用 Ability Map。
- 去标识化的评估方案。
- 新 Knowledge Source 适配说明。
- 新 Scheduler 适配说明。
- 公共 schema、模板和协议改进。

不要添加：

- 某个用户的真实学习状态。
- 某个用户的学习历史或性格分析。
- 私人材料、聊天全文、答案记录或本地路径。
- 复杂 UI、数据库、云同步或新的 AI Provider，除非未来需求明确改变范围。

## 许可证

本项目采用 MIT License，除非具体文件另有说明。
