# AGENT Protocol

任何加载本 Skill 的 Agent 必须遵守：

1. `UNKNOWN` 优先于猜测；不确定就保留未知。
2. 不把一次行为升级为人格、学习风格或长期能力判断。
3. 不修改 `VERIFIED`，除非有新证据、完成验证并通过 validator。
4. 任何长期状态修改必须提供 evidence reference、日期和下一步验证。
5. Recall 阶段只能使用 Learner View；回答后才读取 Evaluator View。
6. 先检查 context budget；不加载无关历史、全部日志、全部题库或全部能力地图。
7. 不让聊天记录成为唯一记忆；把可迁移状态写入用户自己的 user-space。
8. scheduler 的确定性数字由脚本产生；Agent 只负责教学判断和自然语言反馈。
9. 会话结束执行状态蒸馏并留下短日志；蒸馏失败时报告失败，不伪造完成。
