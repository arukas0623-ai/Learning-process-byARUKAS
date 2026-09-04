# Personal Learner State

这是个人学习状态，不是新的学习平台。Chat 只承载本次教学；这里的文件才是可迁移、可审计的长期记忆。

## 边界

- `mastery-loop`：负责主动回忆项目、FSRS 调度、学习队列和基础统计。
- `IRLA`：规定每次交互必须经过 Input → Recall → Learning Validation → Application。
- `Ability Promoting`：复用现有 Obsidian 能力地图，记录能力对象、证据与下一步，而不复制知识库。
- Codex：负责教学、Feynman 追问、反例、语义评价和更新建议；不得用语气或“看过资料”冒充能力证据。

## 每次学习的最小加载包

先运行 `context_budget.py --root <learner-state>` 检查预算，再按其 `load_plan` 读取：`learner.md`、`constraints.md`、与当日题目相关的 `competencies.json` 摘要，以及 `items.json` 的 Learner View 队列。`map.md`、原始证据和历史日志只按当前任务需要查询。

## 状态文件

- `learner.md`：学习目标、已观察的表现和教学有效策略。
- `constraints.md`：由多次观察证实的教学硬规则。
- `competencies.json`：能力评分、事实、假设、验证和置信度。
- `map.md`：当前 C 基础的学习地图；状态由 scheduler 统计更新。
- `items.json`：mastery-loop 引擎所有；不得手改。
- `context-budget.json`：上下文总预算、分文件上限和加载优先级。
- `state-update-policy.md`：长期状态的证据门槛与写入顺序。
- `logs/`：短会话摘要；保留原回答或代码的路径，不把完整聊天复制进来。
- `ability-map-link.md`：到既有 APKM 能力地图与评估协议的稳定引用。

## 状态写入不变量

1. `observed_facts` 只写可追溯行为、回答、代码或测试结果。
2. `model_hypotheses` 不是事实；必须有待执行验证，且可被推翻。
3. 能力等级只由当次或可定位的历史证据更新；无证据则保持 `UNKNOWN`。
4. `transfer_score` 必须来自未见情境，不能由已学题复述代替。
5. 任何长期规则都要有 `evidence_refs`；没有重复证据不进入 `constraints.md`。
6. `due` 阶段只使用 Learner View；用户回答后才调用 `reveal` 获取 Evaluator View。
7. 预算超限时按 `context-budget.json` 的优先级裁剪，不递归读取日志、草稿、原始题库或全部历史；Recall 阶段不得直接打开 `items.json`。

## 常用入口

```powershell
# 学习前预算检查
python C:\Users\35074\.codex\skills\mastery-loop\scripts\context_budget.py --root .

# 当日队列
python C:\Users\35074\.codex\skills\mastery-loop\scripts\scheduler.py --store .\items.json due

# 当前状态统计
python C:\Users\35074\.codex\skills\mastery-loop\scripts\scheduler.py --store .\items.json stats

# 状态证据门槛
python C:\Users\35074\.codex\skills\mastery-loop\scripts\validate_state.py --state .\competencies.json
```

学习者无需运行命令；Codex 在学习会话中执行它们。
`distill` 会自动验证同目录的 `competencies.json`；若存在不满足证据门槛的 `VERIFIED` 条目，蒸馏会被拒绝。
