# 快速开始

## 目录准备

从 `user-space/` 或 `examples/template-learning/` 模板复制一个用户状态目录：

```text
<private-user-space>/learner-state/
```

模板文件使用 `.example` 后缀。复制后请按运行时名称重命名，例如把
`learner.md.example` 改为 `learner.md`，把 `competencies.example.json`
改为 `competencies.json`；真实文件应位于私有 User Space。

用户状态至少要能定位以下内容：

```text
learner.md          # 当前目标、观察到的学习行为、有效策略
constraints.md      # 有重复证据的教学约束
competencies.json   # 能力、观察、假设、验证、证据、置信度
context-budget.json # 总预算与各组件上限
items.json          # scheduler 管理的题目与复习状态
```

## 给 Codex 或其他 Agent 的启动指令

```text
加载 agent-learning Skill。
使用当前目录的 agents/AGENT_PROTOCOL.md 和 Workflow.md。
用户状态位于 <path-to-learner-state>。
知识源为 <source>，能力地图为 <ability-map-provider>，调度器为 <scheduler-adapter>。
先执行 context budget 检查；不要递归读取 logs、全部历史、全部题库或完整能力地图。
按 IRLA 运行一次学习会话，Recall 阶段只展示 Learner View。
```

## mastery-loop 参考适配器

参考配置使用外部 mastery-loop Skill。常见命令形式如下（具体脚本路径以本机安装位置为准）：

```powershell
python <mastery-loop>/scripts/context_budget.py --root <learner-state>
python <mastery-loop>/scripts/scheduler.py --store <learner-state>/items.json due
python <mastery-loop>/scripts/scheduler.py --store <learner-state>/items.json reveal --id <id>
python <mastery-loop>/scripts/scheduler.py --store <learner-state>/items.json grade --id <id> --grade <0-5> --confidence <1-5>
python <mastery-loop>/scripts/validate_state.py --state <learner-state>/competencies.json
python <mastery-loop>/scripts/scheduler.py --store <learner-state>/items.json distill --learner "..." --constraint "..."
```

Recall 阶段只能看到 `due` 返回的 `id`、`front`、`topic`、`difficulty`；用户回答后才调用 `reveal`。

## 会话结束检查

- 是否记录了具体回答、代码、测试或复测位置？
- 是否把观察、假设和稳定结论分开？
- 是否有陌生情境的应用或迁移证据？
- 是否运行状态 validator？
- 是否更新 scheduler 并留下短摘要？

没有完成这些检查时，保持状态为 `UNKNOWN`/`HYPOTHESIS`，不要宣称掌握。
