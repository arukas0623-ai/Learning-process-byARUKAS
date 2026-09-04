# Context Budget Contract

每次会话先检查预算，再按以下优先级加载：

1. 当前任务与当前题目
2. `learner.md`
3. `constraints.md`
4. 当前相关 competency 摘要
5. 历史证据按需查询

默认禁止递归读取 `logs/`、全部历史、全部题库和全部能力地图。超过预算时省略低优先级内容并报告裁剪；不得静默扩大上下文。预算文件至少声明总上限、各组件上限和 `forbidden_defaults`。
