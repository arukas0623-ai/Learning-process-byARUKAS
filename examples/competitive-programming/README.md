# Competitive Programming Learning Instance

这是当前 Learning Skill Framework 的一个实例，目标是准备蓝桥杯和 ACM-ICPC，同时建立长期算法竞赛与软件工程能力。

本实例不绑定具体教材、课程、题库或训练平台，也不把“准备比赛”简化为刷题数量。

## 文件

- `ability-map.json`：可替换的 ACM/蓝桥杯能力地图，覆盖 Phase 0 到 Phase 5。
- `initial-learner-state.json`：按 Evidence Gate 建立的初始学习者状态；未知能力明确保持 `UNKNOWN`。
- `baseline-assessment.json`：第一次 ICPC 能力诊断规格，按 IRLA 测试程序思维、C 基础、Debug 和问题拆解，并规定结果回写 Learner State。
- `baseline-assessment-plan.md`：第一次真实能力诊断，包含 Input、Recall、Learning Validation、Application 和 Transfer。
- `curriculum-generation-protocol.md`：根据证据、误解和迁移结果动态生成下一步的路由规则。

## 评估方式

评估不测试孤立记忆，而观察能否追踪程序状态、解释因果、拆解问题、独立 Debug、写出最小实现并迁移到陌生约束。

长期状态只能遵循：

`Observation → Evidence → Hypothesis → Verification → Stable State`

一次正确或错误回答只形成局部证据，不直接形成长期能力判断；没有足够证据时保持 `UNKNOWN`。

## IRLA 使用方式

- **Input**：提供短材料、程序或问题，明确范围和前置假设。
- **Recall**：关闭材料后闭卷解释执行过程；只使用 Learner View。
- **Learning Validation**：追问原因、边界、反例和误解；回答后才读取 Evaluator View。
- **Application**：编写、修复、拆解或测试一个最小实际任务。
- **Transfer**：改变数据、约束或表述，检查陌生情境中的迁移。

## 可替换边界

- **Ability Map**：可替换为其他竞赛、工程或学科能力地图，只需保留稳定节点和可观察能力边界。
- **Knowledge Source**：可替换为官方文档、公开题面、个人笔记或其他合法来源；来源本身不授予掌握等级。
- **Scheduler**：可替换为 FSRS、其他间隔调度器或人工调度；它只负责复习时间、interval、lapse 等确定性状态。

## 边界

本实例不修改 Core，不引入 UI，不创建数据库，不生成固定长期课程表；实际学习者数据、会话日志和答案应放在私有 User Space，而不是公共实例目录。

本次只建立实例，不开始教学，不生成第一道题。
