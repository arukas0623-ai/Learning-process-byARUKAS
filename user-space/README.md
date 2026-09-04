# User Space

这里放用户拥有的、可迁移的长期状态；不放 Core 规则，也不绑定 Agent。推荐结构：

```text
user-space/
├── learner-profile/   # learner.md、constraints.md
├── selected-goals/    # 当前目标与能力地图选择
├── evidence/          # 可定位的回答、代码、测试和复测证据
└── daily-output/      # 短会话摘要与当前队列输出
```

实际项目可把这些目录合并为一个用户选择的 `learner-state/`，只要保持 Core 的字段和边界。
