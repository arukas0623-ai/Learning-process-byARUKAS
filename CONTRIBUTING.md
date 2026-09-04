# Contributing

感谢参与 `Agent Learning Skill Framework`。本项目优先维护小而稳定的
协议、可迁移的用户状态和可审计的适配契约，不以功能数量为目标。

## Core 修改原则

- Core 必须保持领域无关，不能直接绑定 APKM、Codex 或 mastery-loop。
- 不改变 IRLA、Learner State Schema、Evidence Gate 或 Context Budget 的
  语义，除非同时提供版本说明和迁移理由。
- 规则应写成可检查的 interface，避免把一次 Agent 行为变成长期能力结论。
- 新规范优先复用 `specification/` 的稳定定义；README 和示例只做说明，
  不复制另一份完整规则。
- 不向 Core 引入 UI、数据库、云同步、多 Agent 编排或商业功能。

## Adapter 扩展

新增 Knowledge Source、Ability Map 或 Scheduler Adapter 时：

1. 在 `adapters/` 下新增文档或实现说明。
2. 遵守对应的 `specification/` 文件。
3. 保留用户数据所有权、Context Budget 和 Learner/Evaluator View 隔离。
4. 说明外部依赖、版本、输入输出和失败行为。
5. 如果只有一个实现，把它视为假设性的 seam；不要为了抽象而增加注册中心。

Ability Map Provider 至少提供 `id`、`name`、`description`、
`prerequisites` 和 `assessment_methods`。Scheduler Provider 必须负责
due、评分和复习数值，不把 interval 计算交给 Agent。

## 文档规范

- 使用清晰、可审计的技术描述，不写产品宣传语。
- 新术语先在 Specification 或 Core 中定义，再在 README 中引用。
- 示例必须使用占位符或公开样例，不提交个人学习状态、日志、证据或绝对路径。
- Markdown 链接使用仓库内相对路径；外部依赖提供官方链接或明确名称。
- 更新行为、接口或版本时同步修改 `CHANGELOG.md` 与 `VERSION.md`。

## 测试与检查

提交前至少执行：

```powershell
git diff --check
python <mastery-loop>/tests/test_scheduler.py
```

并检查：

- 所有公开 JSON 可解析；
- Core 没有 APKM、mastery-loop 或 Codex 依赖；
- 没有 `learner.md`、`constraints.md`、`competencies.json`、日志、证据、
  快照或个人绝对路径进入公开目录；
- 新增 Adapter 与对应 Specification 一致；
- 文档中的路径和命令在干净 Clone 中仍然可理解。

本仓库当前是文档优先的 Framework Alpha。外部 mastery-loop 测试不能替代
完整 Framework Runtime 测试；测试结果应如实注明覆盖范围。

## Pull Request 建议

每个 PR 聚焦一个可审查的变化，描述：

- 解决的问题和不解决的问题；
- 受影响的 Core、Adapter 或 User Space；
- 证据、测试命令和结果；
- 是否需要版本、迁移或许可证说明。

不要在一个 PR 中同时引入新运行时、UI、数据库或多个无关抽象。
