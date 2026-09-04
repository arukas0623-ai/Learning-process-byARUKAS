# IRLA Learning Protocol v0.1

## 目标

把“我看懂了”的主观感受，转化为可审查的回答、反例、作品或复测证据。每个循环只推进一个小能力单位，避免大段讲解替代学习。

## I — Input

输入必须标明来源：教材、文档、视频、项目、AI 解释或已有 APKM 节点。记录概念、前置、未解问题和连接，但此阶段不授予能力等级。

## R — Recall

关闭资料后先作答。至少覆盖：它是什么、为什么这样设计、解决什么问题、与什么相连。Codex 只展示题面；回答后才反馈。回忆项由 mastery-loop 排程。

## L — Learning Validation

Codex 是验证者：要求因果链、前提、边界、反例和 Feynman 式解释；可连续追问，最多三次渐进提示后直接补齐前置。评价分为：

- `observed_fact`：原回答或可复现行为说明了什么。
- `model_hypothesis`：对知识缺口的可证伪解释。
- `verification`：下一道具体题、最小实验或代码任务。
- `confidence`：对该判断的 0–1 置信度，不等于学习者自信。

## A — Application

用代码、设计、调试或陌生问题检验迁移。只有陌生约束下仍能说明选择、边界和失败路径，才可以提高 `transfer_score`。

## 能力等级

| 等级 | 可观察标准 |
|---|---|
| L1 | 能准确复述概念，且不泄露答案后完成 |
| L2 | 能解释机制、因果链和一个边界 |
| L3 | 能在熟悉任务中独立应用并给出可检验结果 |
| L4 | 能在未见任务中迁移，说明同构与新增边界 |
| L5 | 能发现错误、提出改进，并用反例或测试支撑 |

## 会话结束

1. 记录 scheduler 的评分与置信度。
2. 将原回答、证据和未解决反例写入短日志或指定能力证据路径。
3. 更新 `competencies.json`，再将真正重复出现的教学规则蒸馏到 `constraints.md`。
4. 调用 mastery-loop `distill`，使缺失的会话蒸馏可见。

## 状态写入门槛

所有长期状态更新遵循 `state-update-policy.md`。写入后运行：

```powershell
python C:\Users\35074\.codex\skills\mastery-loop\scripts\validate_state.py --state .\competencies.json
```

只有通过证据门槛的条目才能标记为 `VERIFIED`；一次错误只能产生带证据引用的观察或待验证假设。
