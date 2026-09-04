# 状态模型与安全边界

## 三类数据必须分离

| 类型 | 回答的问题 | 示例 |
|---|---|---|
| Knowledge Source | 外部材料说了什么？ | 教材章节、PDF、文档、代码仓库 |
| Ability Map | 需要掌握什么能力？ | 节点、前置、能力动作、难度 |
| Learner State | 这个人现在有什么证据？ | 回忆分数、迁移任务、错误、假设 |

知识源不能直接写 VERIFIED；能力地图不能冒充学习证据；Learner State 不能反向改写教材事实。

## 状态升级链

```text
Observation → Evidence → Hypothesis → Verification → Stable State
```

- **Observation**：一次可观察行为，例如“回答中遗漏了 NULL 检查”。
- **Evidence**：可定位的回答、代码、测试、日期和任务标识。
- **Hypothesis**：可证伪的解释，例如“可能不了解动态内存生命周期”。
- **Verification**：下一道闭卷题、陌生变式、实验或复测。
- **Stable State**：只有证据、验证结果和足够置信度都满足时才可标记 `VERIFIED`。

## Answer Isolation

Recall 使用 Learner View；它只含题面和路由元数据。Evaluator View 才包含标准答案、解释、来源和评分标准，并且必须在用户回答后获取。Agent 不得为了“方便批改”预先读取 `items.json` 的答案字段。

## Context Budget

预算文件应至少声明：总上限、画像上限、约束上限、当日题目上限、相关 competency 上限和禁止默认加载的目录。超限时按优先级裁剪并明确报告，不得静默扩大上下文。

## 人格化推断禁令

不得从一次表现推断“懒惰、聪明、视觉型、基础差”等人格或学习风格。可以记录行为模式，但必须有重复证据、来源引用和可推翻的验证方案。
