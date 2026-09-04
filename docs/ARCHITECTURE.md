# 架构说明

## 总体关系

```text
Knowledge Source
      │  source-import adapter
      ▼
Ability Map Provider ───────┐
      │                     │ node IDs / prerequisites
      ▼                     ▼
Learner State ◄──── Agent Learning Protocol ────► Scheduler Adapter
      │                     │ Learner/Evaluator View
      ▼                     ▼
User-owned files       Codex / Claude / local model
```

## 四个边界

### Core

放置所有领域无关、跨 Agent 必须一致的规则：IRLA、状态 schema、evidence rules、Recall/Transfer/Misconception 评估和 context budget。

### Adapters

放置外部系统的接口和实现说明。Adapter 可以替换，但不得绕过答案隔离、证据门槛或用户数据所有权。

### User Space

放置用户目标、画像、能力状态、证据和每日摘要。User Space 可以在不同机器和 Agent 之间复制；不要把聊天导出作为唯一迁移格式。

### Examples

放置真实实例，用来验证框架，而不是作为通用规则来源。当前 `examples/personal-learning/` 是 C 基础 + APKM + mastery-loop 的实例。

## 数据流

```text
Source → extract / cite → Ability Map → select goal
     → plan small competency → Learner View recall
     → user answer → Evaluator View → grade / explain
     → transfer task → evidence → state gate → schedule next review
```

## 责任分工

| 责任 | Owner |
|---|---|
| 解释、追问、反例、Feynman 检查 | Agent |
| 评分语义与错误分类 | Agent |
| 状态字段和证据门槛 | Core policy + validator |
| due date、interval、lapse、FSRS 数值 | Scheduler |
| 来源引用和能力节点 | Source / Ability Map adapter |
| 长期数据保存、迁移、删除 | User |
