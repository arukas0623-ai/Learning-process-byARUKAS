# Agent Learning Skill Framework

## What is this

`Learning-process-byARUKAS` 是一个面向 AI Agent 的个人学习能力管理
Skill Framework。它把教学交互、能力结构和学习者长期状态分开，让不同
Agent 可以读取同一份用户资产，并在不同知识领域中执行可审计的学习循环。

它不是学习 App、题库、课程平台或聊天记录归档工具。

## Architecture

```text
Knowledge Source → Ability Map Provider → Learner State
        (外部知识)       (需要掌握什么)        (现在掌握到哪里)

Agent ↔ Core learning protocol ↔ Scheduler Adapter

Core | Adapters | User Space
```

- `core/`：领域无关的 IRLA、Learner State、评估、证据门槛和上下文预算。
- `adapters/`：Knowledge Source、Ability Map 和 Scheduler 的替换契约。
- `user-space/`：用户状态的公开模板；真实状态应放在本地私有目录。
- `agents/`：任何接入 Agent 都必须遵守的协议。
- `examples/template-learning/`：不含真实数据的公开示例模板。
- `private/`：本地私有实例目录，已被 Git 忽略，不属于公开框架。

## Design Principles

- Evidence over assumption：证据优先于假设。
- UNKNOWN over guessing：没有证据时保持 `UNKNOWN`。
- Context bounded：按预算加载状态，不递归读取全部历史。
- User data ownership：Learner State 是用户资产。
- Domain independent：Core 不绑定学科、Agent 或供应商。
- Answer isolation：Recall 只接收 Learner View，答案在作答后才可读取。

## Core workflow

```text
Source Input → Ability Extraction → Planning → Recall
→ Correction → Application / Transfer → Evidence
→ Learner State Update → Future Scheduling
```

IRLA 定义为 Input → Recall → Learning Validation → Application。长期状态
只能沿 Observation → Evidence → Hypothesis → Verification → Stable State
更新；一次正确或错误回答都不能直接代表长期能力。

## Reference adapters

| Role | Current reference | Replaceable with |
|---|---|---|
| Agent | Codex | Claude Code、本地模型或其他能执行协议的 Agent |
| Ability Map | APKM | 手工技能树、课程地图或其他 Provider |
| Scheduler | mastery-loop + FSRS | 其他支持 due、评分和复习状态的本地调度器 |
| State | Markdown + JSON | 保持字段和证据契约的其他文件布局 |

Core 不依赖 APKM、mastery-loop 或 Codex；这些依赖只存在于 adapters 和
具体实例。当前 mastery-loop 运行时是外部 Skill，不随本仓库打包。

## Public template and private data

从 `examples/template-learning/` 复制模板到本地 User Space，再替换知识源、
能力节点和调度器配置。真实的 `learner.md`、`constraints.md`、
`competencies.json`、回答、日志、快照和绝对路径不得提交到公开仓库；本地
`private/` 目录已加入 `.gitignore`。

个人学习只是该框架的一个实例，不是 Core 规则来源。公开模板不包含个人
学习历史、答案或证据。

## Status and scope

- 当前版本：见 [VERSION.md](VERSION.md)，`0.2.0-alpha`。
- 当前成熟度：Personal Stable / Framework Alpha，适合受控个人或小规模试运行。
- 不提供 UI、数据库、云同步、商业功能或多 Agent 编排。
- 适配器目前以文档契约和外部工具为主，不是完整插件注册系统。
- MIT License，见 [LICENSE](LICENSE)。

## Quick Start

1. Clone this repository。
2. 复制 `examples/template-learning/` 到自己的私有 User Space，并把
   `.example` 文件名改为运行时需要的 `learner.md`、`constraints.md`、
   `competencies.json`、`context-budget.json` 和 `items.json`。
3. 选择 Knowledge Source 和 Ability Map Provider。
4. 选择 Scheduler Adapter，并确认其支持 Learner View / Evaluator View。
5. 让 Codex、Claude Code 或本地模型加载 `SKILL.md` 和
   `agents/AGENT_PROTOCOL.md`。
6. 先运行预算检查，再开始一次小范围 IRLA 学习循环。

## Start an Agent session

让 Agent 加载根目录的 `SKILL.md`、`agents/AGENT_PROTOCOL.md` 和选定的
User Space，然后发送：

```text
加载 agent-learning Skill，读取指定的 User Space、知识源、能力地图和调度器。
先执行 context budget 检查，Recall 阶段只展示 Learner View，按 Workflow.md
完成一次 IRLA 学习循环，并只写入有证据链的状态更新。
```

一个不依赖特定 Agent 的接入说明见 [docs/QUICKSTART.md](docs/QUICKSTART.md)。

## Example Workflow

一次会话只加载当前任务需要的状态：

```text
Agent loads:
  learner state
  constraints
  due items (Learner View)

User answers
Agent evaluates (Evaluator View)
Application / transfer task runs
Evidence is recorded
Learner State is updated only through the evidence gate
```

无个人数据的完整示例见 [examples/demo-flow/README.md](examples/demo-flow/README.md)。

## Documentation

- [SKILL.md](SKILL.md)：Skill 入口和硬性边界
- [Workflow.md](Workflow.md)：完整学习循环
- [agents/AGENT_PROTOCOL.md](agents/AGENT_PROTOCOL.md)：Agent 执行协议
- [CONTRIBUTING.md](CONTRIBUTING.md)：贡献、Adapter 扩展和测试规范
- [ROADMAP.md](ROADMAP.md)：已完成、计划和明确不做的方向
- [VERSION.md](VERSION.md)：版本和验证里程碑
- [specification/](specification/)：稳定的 Learner State、Ability Map、Evidence 和 Adapter 接口
- [docs/PROJECT.md](docs/PROJECT.md)：项目定位、目标和非目标
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)：Core、Adapter、User Space 关系
- [docs/STATE_MODEL.md](docs/STATE_MODEL.md)：状态模型、证据门槛和答案隔离
- [docs/ADAPTERS.md](docs/ADAPTERS.md)：当前适配契约
- [docs/MAINTENANCE.md](docs/MAINTENANCE.md)：备份、迁移和隐私维护
- [framework-manifest.json](framework-manifest.json)：机器可读入口清单

## Contribution boundary

优先修复可观察的协议缺口、答案泄漏、证据污染、上下文失控和迁移问题。
不要因为功能数量而把 UI、数据库、云同步、排行榜或多 Agent 编排加入 Core。

## License

代码与文档采用 MIT License。外部依赖仍受其各自许可证约束。
