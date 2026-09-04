# Ability Map Adapter

Ability Map Provider 回答“需要掌握什么”，不回答“学习者现在掌握多少”。它至少输出稳定节点 ID、标题、前置关系、难度、能力动作和来源引用。

APKM 是当前实例的 Provider，不是 Core 依赖。其他实现可以用课程目标、技能树或手工 Markdown；替换 Provider 不得改变 Learner State schema 或 evidence rules。
