# 适配器说明

## Knowledge Source Adapter

输入可以是教材、PDF、课程、GitHub 项目、文档或笔记。输出应保留 `source_id`、版本/日期、路径、章节范围和提取方法。适配器只提供可引用材料，不直接授予能力等级。

## Ability Map Adapter

输出稳定节点 ID、标题、前置关系、难度、能力动作和来源引用。APKM 是当前 Provider；课程技能树、手工 Markdown 或其他知识图谱也可以实现同一契约。Provider 只描述“要掌握什么”，不描述“已经掌握多少”。

## Scheduler Adapter

至少需要支持：

- 获取当日到期项目；
- Learner View / Evaluator View 分离；
- 记录评分与置信度；
- 更新间隔、lapse 和下一次复习；
- 输出可审计的历史或统计；
- 不让 Agent 手算或猜测时间。

当前 mastery-loop 提供 FSRS/SM-2、interleaving、calibration、hypercorrection 和本地 JSON 存储。替换 scheduler 时，Learner/Evaluator View 和状态更新边界不能改变。

## Agent Adapter

Agent 不需要特定 SDK。只要能够读取 Markdown/JSON、执行必要脚本并遵守 `agents/AGENT_PROTOCOL.md`，就可以接入 Codex、Claude Code 或本地模型。

## 替换顺序

先替换一个 adapter 并运行同一组 recall、transfer、evidence 和 budget 验证；不要同时更换所有层，否则无法判断行为变化来自哪里。
