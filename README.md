# Agent Learning Skill Framework

这是一个可插拔的 Agent 学习 Skill 基础，不是学习平台。Agent 负责教学、提问、纠错与判断；文件和确定性调度器负责持久化、证据与复习时机。

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

## 迁移原则

只要目标 Agent 能读取 Markdown/JSON 并调用所选 scheduler，就可以迁移；不得要求导入聊天记录。替换知识源或能力地图不会改变 Core 的证据门槛、答案隔离和预算规则。

本仓库名为 `Learning-process-byARUKAS`，用于保存个人学习过程与可复用的 Agent Learning Skill 框架。
