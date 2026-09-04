# Dynamic Curriculum Generation Protocol

本协议生成“下一步诊断或训练动作”，不生成预先排好的长期课程表。

## 输入

每次路由只使用：

- competency 的 `status`、`confidence` 和 `verification_required`；
- 已记录的 Evidence、Observation、Hypothesis 和 misconception；
- 最近一次 Application 与 Transfer 的结果；
- scheduler 提供的到期 Learner View 和间隔数据。

Knowledge Source、Ability Map 和 Scheduler 都是可替换组件；本协议不假设某一本教材或某个平台。

## 路由规则

| 当前证据 | 下一步动作 |
| --- | --- |
| 基础概念为 `UNKNOWN` | 先做最小闭卷诊断；若前置缺失，再提供短 Input，不批量推进后续阶段。 |
| 状态为 `HYPOTHESIS` | 针对假设设计验证题，优先检查因果、边界和反例。 |
| 概念达到 `VERIFIED` | 进入小型 Application，再进入陌生 Transfer；不因为一次正确就跳过验证。 |
| 同一误解重复出现 | 建立 misconception 记录，生成同构但改变表面情境的针对性训练，并复测原边界。 |
| Transfer 可复现成功 | 增加约束、规模、组合复杂度或减少提示，逐级提高难度。 |
| Recall 正确但 Application 失败 | 保留概念理解与实现能力的区别，回到最小实现和 Debug 验证。 |
| Application 正确但 Transfer 失败 | 不升级迁移能力，改变题面或约束后继续验证模型边界。 |
| 证据冲突或不足 | 保持 `UNKNOWN`，优先收集新证据，不用平均分掩盖未知。 |

## 阶段切换条件

阶段不是按时间或完成题数切换，而是按前置节点的证据切换：

1. 前置节点必须有可指向的 Evidence。
2. 至少一个 Application 结果可复核。
3. 至少一个陌生 Transfer 或等价验证完成。
4. validator 通过，且没有未处理的关键边界误解。
5. 未满足条件时只推进当前最小缺口，不宣布进入下一阶段。

`VERIFIED` 只能在 Evidence、Verification 和置信度门槛都满足时写入；否则保持 `UNKNOWN` 或 `HYPOTHESIS`。

## 每轮闭环

完成 IRLA 与 Transfer 后：

1. 写入 Observation 和 Evidence 引用。
2. 写入可证伪 Hypothesis 与下一项 Verification。
3. 运行 learner-state validator。
4. 由 Scheduler 负责 grade、lapse、interval 和 next review。
5. 执行 distill，留下简短会话摘要。

Agent 负责提问、解释、追问和判断；Scheduler 不负责解释能力，也不替代 Evidence Gate。
