# Learning Workflow

```text
Source Input
  ↓
Ability Extraction
  ↓
Learning Planning
  ↓
Recall（Learner View）
  ↓
Correction（Evaluator View after answer）
  ↓
Application / Transfer Test
  ↓
Evidence Collection
  ↓
Learner State Update（Evidence Gate）
  ↓
Future Scheduling
```

## 操作契约

1. **Source Input**：标记来源、版本和范围；没有来源时标记为 AI-sourced。
2. **Ability Extraction**：从材料提取“需要能够做什么”，不把资料目录当能力地图。
3. **Planning**：按前置关系拆成小能力单元，并给每个单元配置 recall、解释、应用或迁移任务。
4. **Recall**：只展示题面；答案、评分标准和解释在用户作答后才读取。
5. **Correction**：指出回答与标准之间的具体不匹配，并要求边界、反例或 Feynman 解释。
6. **Application**：使用未见情境；迁移能力不得由原题复述代替。
7. **Evidence**：保存回答、代码、测试或复测位置；事实、假设、验证结果分开。
8. **State Update**：通过 evidence validator 后再更新稳定状态；否则保持 `UNKNOWN` 或 `HYPOTHESIS`。
9. **Scheduling**：只由 scheduler 更新 due date、interval、lapses、FSRS/SM-2 数值。

会话结束必须留下短摘要和下一步；原始聊天不复制进长期状态。
