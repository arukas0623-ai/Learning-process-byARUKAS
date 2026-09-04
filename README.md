# Agent Learning Skill Framework

`Learning-process-byARUKAS` 是一个以文件为中心、可插拔、面向长期学习的 Agent Skill 框架。它把聊天中的教学交互与用户自己的 Learner State 分开：Agent 负责提问、解释、挑战和评价；本地 Markdown/JSON 与确定性调度器负责证据、历史、复习时间和迁移。

它不是 Web 学习平台、笔记应用、题库产品或多 Agent 系统。目标是让 Codex、Claude Code 或本地模型能够加载同一套学习协议，同时更换教材、能力地图、调度器和用户状态。

## 项目状态

- **版本**：`0.1.0`（框架抽象阶段）
- **成熟度**：可用于个人实例和小规模试运行；尚未宣称生产级平台
- **存储**：用户拥有的 Markdown + JSON；不要求云服务或数据库
- **当前调度器**：外部 `mastery-loop` Skill，默认 FSRS
- **当前能力地图**：个人实例使用 APKM；Core 不依赖 APKM
- **许可证**：当前仓库尚未声明开源许可证；在明确许可证前，不应假设可再发布或商业复用

## 架构

```text
Knowledge Source → Ability Map Provider → Learner State
                         ↑                    ↑
                 可替换 adapter        用户拥有的 Markdown/JSON

Agent ↔ IRLA / Evaluation ↔ Scheduler Adapter
```

目录职责：

- `core/`：领域无关的学习协议、状态规则、评估和上下文边界。
- `adapters/`：知识源、能力地图和调度器的替换契约；当前实现使用 APKM 与 mastery-loop。
- `user-space/`：实际用户状态的承载位置，不绑定 Codex、Claude 或项目路径。
- `agents/`：任何 Agent 必须遵守的操作协议。
- `examples/personal-learning/`：当前个人系统的第一个实例，不是 Core。

`user-space/context-budget.json` 是可复制的预算模板；个人实例当前使用 `examples/personal-learning/learner-state/context-budget.json`。

## 可替换边界

| 层 | 当前实现 | 可替换内容 |
|---|---|---|
| Knowledge Source | 本地教材/文档/笔记 | PDF、课程、GitHub 项目、手工 Markdown 或其他导入器 |
| Ability Map | APKM | APKM、课程技能树、手工能力地图 |
| Scheduler | mastery-loop + FSRS | 其他本地调度器；必须保留 Learner/Evaluator View 契约 |
| Learner State | Markdown + JSON | 文件布局可变，但证据字段、状态门槛和用户所有权不变 |
| Agent | Codex | Claude Code、本地模型或其他能执行协议的 Agent |

## 当前个人实例

`examples/personal-learning/learner-state/` 保存 C 基础的个人状态、题库、能力引用和预算。进入该目录后，Agent 先执行预算检查，再通过 mastery-loop `due` 取得 Learner View；用户回答后调用 `reveal`、`grade`，按证据协议更新状态并 `distill`。这只是实例配置，不会被 Core 当作通用知识。

## 使用方式

将一个用户自己的 `user-space/` 目录与所选 adapters 提供给任意 Agent，然后要求：

```text
加载 agent-learning Skill，读取我的 user-space 状态，使用配置的知识源、能力地图和调度器，按 IRLA 开始一次学习会话。
```

一次性解释、翻译和总结不应启动长期学习循环。

## 快速开始

1. 准备一个用户状态目录，至少包含 `learner.md`、`constraints.md`、`competencies.json`、`context-budget.json` 和调度器所需的 `items.json`。
2. 让 Agent 加载根目录的 `SKILL.md` 与 `agents/AGENT_PROTOCOL.md`。
3. 指定知识源、Ability Map Provider、Scheduler Adapter 和用户状态目录。
4. 要求 Agent 执行：

   ```text
   加载 agent-learning Skill，读取指定的用户状态，按 Workflow.md 执行一次长期学习会话；先检查 context budget，Recall 阶段只使用 Learner View。
   ```

5. 会话结束时，Agent 必须保存证据、运行状态校验、更新复习状态并留下短摘要。

如果只是问一个孤立问题，不要加载此 Skill；直接回答即可。

## 迁移原则

只要目标 Agent 能读取 Markdown/JSON 并调用所选 scheduler，就可以迁移；不得要求导入聊天记录。替换知识源或能力地图不会改变 Core 的证据门槛、答案隔离和预算规则。

本仓库名为 `Learning-process-byARUKAS`，用于保存个人学习过程与可复用的 Agent Learning Skill 框架。

## 文档索引

- [SKILL.md](SKILL.md)：Agent Skill 入口与硬性边界
- [Workflow.md](Workflow.md)：完整学习循环
- [agents/AGENT_PROTOCOL.md](agents/AGENT_PROTOCOL.md)：所有 Agent 的执行协议
- [docs/PROJECT.md](docs/PROJECT.md)：项目定位、目标与非目标
- [docs/QUICKSTART.md](docs/QUICKSTART.md)：从零开始接入一个 Agent
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)：Core、Adapter、User Space 和实例关系
- [docs/STATE_MODEL.md](docs/STATE_MODEL.md)：Learner State、证据门槛与答案隔离
- [docs/ADAPTERS.md](docs/ADAPTERS.md)：知识源、能力地图和调度器适配契约
- [docs/MAINTENANCE.md](docs/MAINTENANCE.md)：备份、迁移、版本和隐私维护
- [framework-manifest.json](framework-manifest.json)：机器可读的入口清单

## 贡献边界

优先修复可观察的协议缺口、数据损坏风险、答案泄漏和上下文失控；不要因为“功能更多”就把 UI、数据库、云同步、排行榜或多 Agent 编排加入 Core。新增规则应附带可复现证据和最小验证。
