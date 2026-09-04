# C 程序推理基础 — Learning Map

| # | Topic (slug) | 标题 | 难度 | 前置 | 为什么重要 | 状态 |
|---|---|---|---|---|---|---|
| 1 | control-flow-invariants | 循环边界与累加器不变量 | intro | — | 是追踪、调试和算法实现的地基 | 🔴 |
| 2 | scope-and-storage | 作用域、对象与变量遮蔽 | intro | control-flow-invariants | 避免“名字相同即对象相同”的错误模型 | 🔴 |
| 3 | input-destinations | `scanf` 的目标地址与未初始化对象 | core | scope-and-storage | 连接输入、指针和未定义行为 | 🔴 |
| 4 | pointer-validity | 指针值、解引用与访问有效性 | core | input-destinations | 建立内存安全的检查模型 | 🔴 |
| 5 | c-transfer | C 概念的陌生问题迁移 | advanced | pointer-validity | 区分会追踪与能独立工程推理 | 🔴 |

## 资料连接

- APKM：`N02-01 语法与类型`、`N02-02 指针与内存`、`N02-03 数组与字符串`。
- 当前焦点：首轮基线回忆；尚未授予能力等级。
