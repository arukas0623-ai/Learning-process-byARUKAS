# IRLA Protocol

IRLA = `Input → Recall → Learning Validation → Application`。

## Input

记录材料来源、范围、核心概念、前置知识、未解问题和与既有能力的连接；输入本身不授予掌握等级。

## Recall

关闭资料后先回答“是什么、为什么、解决什么、与什么相连”。Recall 阶段只能使用 Learner View：`id`、`front`、`topic`、`difficulty` 等非答案字段。

## Learning Validation

Agent 追问因果链、前提、边界、反例和 Feynman 式解释；回答后才加载 Evaluator View，并记录评分、置信度和错误类型。

## Application

通过代码、设计、调试或陌生约束验证迁移。只有陌生情境中可复现的选择、边界和失败路径，才能支持 transfer evidence。
