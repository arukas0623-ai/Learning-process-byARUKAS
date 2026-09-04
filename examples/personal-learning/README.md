# Personal Learning Example

这是本 Skill 的第一个实例。原有个人 `learner-state/` 已整体保留在本目录，继续使用 IRLA、APKM 能力地图和 mastery-loop 调度器；它属于用户数据，不属于 Core。

使用其他领域时，复制状态结构并替换知识源与 Ability Map adapter，不要复制本实例的 C 语言目标、个人事实或绝对路径。

## 运行顺序

1. 将本目录的 `learner-state/` 作为用户状态目录交给任意兼容 Agent。
2. 运行 `context_budget.py`，读取预算允许的状态和 mastery-loop Learner View。
3. 按 IRLA 进行 Input、Recall、Learning Validation、Application。
4. 回答后才请求 Evaluator View，完成评分与证据记录。
5. 运行状态 validator，再执行 scheduler `distill`；不满足证据门槛时保持 `UNKNOWN`/`HYPOTHESIS`。
