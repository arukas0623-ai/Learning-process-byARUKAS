# Demo Flow

这是一个不含个人数据的公开流程示例。它使用一个简短的 C language
basics sample document 和手工 Ability Map，故意不绑定 APKM、Codex 或
mastery-loop 的具体实现。

## Flow

```text
Knowledge Source
        ↓
Ability Map Adapter
        ↓
Learning Item Generation
        ↓
IRLA Loop
        ↓
Recall（Learner View）
        ↓
Validation（Evaluator View after answer）
        ↓
Application Test
        ↓
Evidence Update
        ↓
Learner State
```

## Walkthrough

### 1. Knowledge Source

Sample document：`C function parameters: values, pointers and observable
side effects`。

Source adapter records a source ID, version/date and selected section. It does
not assign a learner level.

### 2. Ability Map Adapter

The provider emits one domain-neutral node:

```yaml
id: c.functions.pointer-side-effects
name: Explain pointer-based side effects
description: Trace how a function can mutate caller-visible state through a pointer.
prerequisites: [c.functions.parameters]
assessment_methods: [recall, explanation, application, transfer]
```

This is a sample provider output, not an APKM export.

### 3. Learning Item Generation

The scheduler receives a small item whose `front` asks:

> What must a C function receive if it needs to change the caller's integer?

The item may contain an Evaluator View internally, but the Recall phase only
receives its ID, front, topic and difficulty.

### 4. IRLA loop

1. **Input**：Agent gives the selected source section or a short explanation。
2. **Recall**：Agent shows only the question and asks the learner to answer from memory。
3. **Validation**：After the answer, Agent loads the evaluator material, asks a
   Feynman explanation and checks an edge case。
4. **Application**：Learner predicts or writes a small function for an unseen
   caller-state mutation task。

### 5. Evidence update

Record the concrete answer, application result and timestamp. Keep the first
observation as `UNKNOWN` or `HYPOTHESIS` unless a verification task supplies
enough evidence for `VERIFIED`.

The demo stops at the state-update decision. It does not contain a real learner
profile, history, log, answer key or persistent personal state.
