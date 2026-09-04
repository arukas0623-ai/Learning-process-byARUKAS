# 面向 Codex 的持久化 Learner State：GitHub 开源项目审计

审计日期：2026-09-03  
目标：把长期、高 Token 的导师式学习对话压缩为本地、可持续评估的 Learner State；不开发新学习系统。  
判定原则：优先“聊天即界面、文件即状态、确定性调度、可验证评估”，不按功能数量或 Star 排名。

## 结论先行

没有一个候选完整覆盖“Codex 原生 + 开放回答评估 + 对抗式追问 + 显式误解模型 + FSRS + 统一长期 Learner Profile + 极低运维”。最接近的是 [all666666all/mastery-loop](https://github.com/all666666all/mastery-loop)。

**当前建议：直接使用 mastery-loop 做真实学习试运行，不先 Fork。** 它的核心边界最符合目标：Codex/Claude Skill 负责导师行为，Python 负责确定性调度，Markdown/JSON 保存用户自己的状态。只有真实使用暴露出重复性缺口后，才值得做极小 Fork；当前没有证据支持先造平台或合并多个项目。

RetainCraft 是第二名，但其学习契约、提醒、倦怠检测、职业对标、周报等流程会增加上下文和仪式感。MindTrain 与 SkillClimb 是成熟度更高的“应用平台”方向，却与“主要在 Codex 中使用、少维护 UI、降低 Token/复杂度”的目标相反。两个名为 Learning Loop 的项目本质上是 AI 会话/知识库的学习与记忆系统，不是对人的能力进行学习评估的系统。

## 项目身份与歧义

- `mastery-loop`：本审计采用 [all666666all/mastery-loop](https://github.com/all666666all/mastery-loop)，因为仓库描述、`SKILL.md`、FSRS 和 Codex 安装路径与问题完全对应。
- `MindTrain`：采用 [shigella520/MindTrain](https://github.com/shigella520/MindTrain)。
- `SkillClimb`：采用 [danallison/skillclimb](https://github.com/danallison/skillclimb)，而非同名空壳仓库。
- `RetainCraft`：采用 [kaixiad/RetainCraft](https://github.com/kaixiad/RetainCraft)。
- `Learning Loop` 有歧义，审计两个最相关候选：
  - [melodykoh/learning-loop-skill](https://github.com/melodykoh/learning-loop-skill)：Claude Code 会话经验捕获 Skill。
  - [robinslange/learning-loop](https://github.com/robinslange/learning-loop)：Claude Code/Codex 知识库与上下文工程插件。
  - [microsoft/learning-loop](https://github.com/microsoft/learning-loop) 是在线强化学习 Joiner/Trainer，不属于个人学习系统，排除。

## 对比表

符号：✅ 已在源码/契约中实现；◐ 部分实现或依赖 LLM 提示纪律；❌ 未实现；? 证据不足。

| 项目 | Codex / Agent 接入 | Learner Profile / 历史 | Active Recall / 开放回答 | 掌握度 / 错误缺口 | SRS | Feynman / 对抗提问 | 本地与数据所有权 | 运维复杂度 | 目标匹配 |
|---|---|---|---|---|---|---|---|---|---:|
| mastery-loop | ✅ 标准 Skill，明确 Codex/Claude | ✅ `learner.md`、`constraints.md`、会话日志 | ✅ 逐题回忆；◐ LLM 评分开放回答 | ✅ 主题掌握度、置信校准、错误驱动复习；◐ 误解靠文本提炼 | ✅ FSRS-5 / SM-2 | ✅ Feynman；◐ 对抗式追问未独立建模 | ✅ Markdown + JSON，本地可移植 | 低：Python 标准库 | **88/100** |
| RetainCraft | ◐ 通用 Agent Skill；README 明列 Claude Code，未给 Codex 专用安装 | ✅ `profile.json`、测试/模拟/活动历史 | ✅ 协议式主动回忆；◐ AI 评分 | ✅ L1-L5、概念准确率、弱项分析；◐ 误解不是一等实体 | ✅ FSRS-5 / SM-2 | ✅ L5 Feynman、因果追问 | ✅ `~/learn` 下 JSON/Markdown | 中：单脚本但流程很宽 | **82/100** |
| SkillClimb | ✅ MCP 可供 Agent；◐ 无 Codex Skill | ✅ learner nodes、review history、journal、profile resource | ✅ free recall + AI evaluation | ✅ IRT、校准、`misconceptions`、progress | ✅ 修改版 SM-2；❌ FSRS | ◐ 反思 journal；Feynman 无硬流程 | ✅ 自托管 PostgreSQL；JSON 导出不含全部状态 | 高：React + Express + Postgres + auth | **70/100** |
| MindTrain | ✅ Codex Plugin + Skill + MCP | ◐ Attempt/Interaction/Mastery 很完整；无丰富叙事画像 | ◐ 主要是单选/多选，确定性判分 | ✅ 主题掌握度与错题历史；◐ 知识缺口偏题库覆盖 | ◐ 加权到期调度；FSRS 尚在路线图 | ◐ 可追问/质疑；无 Feynman 状态 | ✅ 私有 PostgreSQL，但不是轻量文件状态 | 高：Core + MCP + Web + Postgres/Docker | **63/100** |
| Learning-loop-skill | ✅ Claude Code Skill | ✅ 捕获 AI 会话经验，不是 learner profile | ❌ | ◐ 追踪工作流失败，不追踪人的知识误解 | ❌ | ◐ 有“对抗审计角色”，但审计的是捕获质量 | ✅ 本地 Markdown/Memory | 中高：多 Agent、路由与门禁 | **38/100** |
| robinslange/learning-loop | ✅ Claude Code，文档也描述 Codex 差异 | ✅ Vault/episodic memory，但不是 learner profile | ❌ | ◐ 知识库 gaps，不是能力缺口 | ❌ | ❌ | ✅ 本地 Vault + `edges.db`，支持离线模式 | 很高：hooks、原生检索、Ollama、20 agents | **32/100** |

评分是面向本目标的加权适配度，不是通用产品质量分。权重更看重状态压缩、导师闭环、评估/错误、调度、本地所有权与低运维。

## 架构分析

### 1. mastery-loop — 最符合“Codex 是界面，文件是状态”

```text
Codex / Claude Skill
  ├─ 读取 learner.md + constraints.md + 当日 due queue
  ├─ 提问、评估开放回答、Feynman 追问、反馈
  └─ 调用 Python scheduler
          └─ items.json（FSRS/SM-2、尝试、置信度、到期日）

map.md + learner.md + constraints.md + logs/*.md
```

[SKILL.md](https://github.com/all666666all/mastery-loop/blob/main/SKILL.md) 明确规定模型只负责语言与判断，脚本负责时间、间隔、lapse、interleaving、mastery 和 calibration；[工作区格式](https://github.com/all666666all/mastery-loop#how-it-works) 是每科一个 Markdown/JSON 文件夹。这个分工正是低 Token 的关键：新任务不需要回放全部对话，只载入画像、约束和当日队列。

真实优点：

- Learner State 不只是卡片：`learner.md` 记录节奏、有效解释方式、错误类型、易混概念和校准趋势；`constraints.md` 把反复错误变成下次必须执行的教学约束。
- 错误会改变未来行为：错题立即纠正、置信错误快速复测、Feynman 暴露的缺口生成新 item。
- [FSRS 实现](https://github.com/all666666all/mastery-loop/blob/main/scripts/fsrs.py) 维护 stability/difficulty/retrievability；[scheduler](https://github.com/all666666all/mastery-loop/blob/main/scripts/scheduler.py) 默认 FSRS，也保留 SM-2。
- 本机运行 `python tests/test_scheduler.py`：**36 passed, 0 failed**。

边界与风险：

- 开放回答评分、误解提炼和 `learner.md` 更新仍依赖 LLM 遵守 Skill；没有强 schema 或审计器阻止画像漂移。
- “掌握度”主要由复习状态推导，不是 IRT 或经过效度验证的能力测量。
- learner profile 默认按学科分散，没有天然的跨学科统一画像。
- 仓库规模很小、截至 2026-06-20 默认分支最后提交，维护历史短；README 的教育效果叙述不能替代真实用户验证。

行动：**直接使用**。先用一个真实主题跑 2–3 周；不因理论缺口预先 Fork。

### 2. RetainCraft — 功能更宽，但会重新制造 Token 与流程负担

```text
Agent Skill（大体量教学协议）
  └─ scripts/srs.py 单体 CLI
       ├─ topics/*/concepts.json
       ├─ profile.json / config.json
       ├─ learning_log.json
       └─ test_history.json / simulation_history.json
```

[README](https://github.com/kaixiad/RetainCraft) 与 [SKILL.md](https://github.com/kaixiad/RetainCraft/blob/main/SKILL.md) 覆盖诊断测试、主动回忆、Feynman、因果追问、模块测试、等级、提醒和周报；[srs.py](https://github.com/kaixiad/RetainCraft/blob/main/scripts/srs.py) 确实持久化 profile、活动日志、测试历史和 FSRS 状态。本机运行其测试：**170 tests, OK**。

它比 mastery-loop 多的功能，大多不是当前核心瓶颈。每次会话的必读检查清单、提醒确认、学习契约、职业比较、倦怠检测、周报与大量理论说明，会占用提示上下文，也增加“协议执行失败”的表面积。其 Feynman/因果追问仍主要由 AI 协议执行；概念错误以统计与日志存在，但没有强类型 misconception/contradiction 状态。源码也明确短期 stability 是完整 FSRS-5 的简化版。

行动：**只借鉴设计**。若用户明确需要正式等级测试或提醒系统，才把它作为直接使用的备选；当前不建议 Fork 合并进 mastery-loop。

### 3. MindTrain — Codex 集成最好，但它是服务，不是轻量 Learner State

```text
Codex Plugin / Skill
   → 本地 bridge
   → Trainer MCP
   → Training Core API
   → PostgreSQL
Web → Training Core API
```

[README](https://github.com/shigella520/MindTrain) 和 [Plugin Skill](https://github.com/shigella520/MindTrain/blob/main/plugins/mindtrain/skills/mindtrain/SKILL.md) 对 Codex 的接入最完整；题目、Session、Attempt、Interaction、Review State 与 Topic Mastery 由 Core 统一持久化，[初始数据库结构](https://github.com/shigella520/MindTrain/blob/main/apps/trainer-core/src/main/resources/db/migration/V1__initial_schema.sql) 可验证这些实体。选择题采用确定性判分，这比 LLM 自评可靠。

但当前核心训练围绕选项题；丰富开放回答、Feynman、自我解释和显式 learner narrative 都不是一等能力。[官方 README](https://github.com/shigella520/MindTrain#为什么使用-mindtrain) 明确 Anki/FSRS Provider 仍在规划，当前只是加权到期/新题调度。部署要求 Docker Compose、Java Core、MCP、Web 和 PostgreSQL；这与“不维护复杂 UI/服务”的目标不符。

运行证据：公开 `ci.yml` badge 在审计时为 passing；本机缺少 Docker，且插件 Python 测试初次执行因缺 PyYAML 停止，所以**没有本机端到端运行通过的证据**，不能把 README 的 5 分钟部署当成已验证事实。

行动：**只借鉴设计**。值得借鉴的是“Skill 无状态、Core 为唯一权威源、写入幂等、版本契约与明确确认”，不建议当前直接部署或 Fork。

### 4. SkillClimb — 评估模型最强，产品边界最重

```text
Agent → MCP → Express API → PostgreSQL
                       ├─ functional core：SM-2 / IRT / scoring / calibration
React UI ──────────────┘
AI Provider：Anthropic / OpenAI / Ollama
```

[README](https://github.com/danallison/skillclimb) 展示的评估能力最完整：IRT placement、五级问题难度、free-recall AI evaluation、confidence calibration、learning journal，以及 MCP learner-profile resource。[数据导出说明](https://github.com/danallison/skillclimb/blob/main/docs/data-export-import.md) 证明 learner nodes、reviews、study days 可导出为 JSON，并包含 `misconceptions`；但 sessions 与 placement results 不导出，所以“可移植 Learner State”并不完整。

它的主要问题不是功能不足，而是边界过宽：React、Express、PostgreSQL、OAuth/JWT、内容树、AI provider、Docker。默认内容以 cybersecurity skill tree 为首个实现，迁移到任意学习领域要维护内容模型。它有通用 MCP，但没有现成 Codex Skill 来稳定执行导师协议。调度为修改版 SM-2，不是 FSRS。

运行证据：源码与大量测试文件存在；本机 `npm ci` 在审计窗口内未完整建立 `vitest` 可执行文件，故**未取得本机测试通过证据**，也不把安装中断解释成项目失败。

行动：**只借鉴设计**。重点借鉴 IRT 初测、置信校准和显式 misconceptions；不建议直接使用或 Fork。

### 5. Learning Loop — 名字接近，问题域不同

#### melodykoh/learning-loop-skill

这是“让 Claude 从工作会话中学习”的 Skill：[README](https://github.com/melodykoh/learning-loop-skill) 描述 scan → capture → consolidate → quality gate → user verification → route。它持久化失败尝试、用户纠正、判断变化和重复流程问题，目标是防止 `/clear`/压缩丢失工程经验。

可借鉴的只有两点：先保存原始证据，再提炼结论；任何长期状态写入前让用户核验。它没有学习题目、作答、掌握度、SRS、Feynman 或能力复测。

行动：**只借鉴设计**，不要当学习系统使用。

#### robinslange/learning-loop

这是本地知识 Vault 与上下文工程平台：[README](https://github.com/robinslange/learning-loop) 描述 Markdown Vault、`edges.db`、episodic memory、hooks、来源验证、检索注入、Ollama librarian，以及 Claude Code/Codex 差异；也支持 `LL_OFFLINE=1`。它能降低“重复搜索/重复解释”类 Token，但保存的是知识与 AI 工作判断，不是人的能力状态。

其 20 agents、hooks、原生检索二进制、Ollama、边图、来源解析器和 Vault 生命周期远超本目标所需。把它引入会用知识管理复杂度替代学习状态复杂度。

行动：**只借鉴设计**；不直接使用，不 Fork。

## 真正有价值的功能

1. **最小可加载状态包**：学习目标/地图、learner profile、硬约束、到期队列、最近关键错误。它直接替代长对话回放。
2. **确定性引擎与 LLM 判断分离**：时间、FSRS、统计、ID、历史由代码处理；Codex 只做提问、解释和语义评分。
3. **先回答后反馈**：每次只展示题面，避免“看懂了”等同于“会了”。
4. **错误成为未来状态**：记录“说了什么、正确是什么、为什么错、何时再测”，而不仅是一个低分。
5. **置信度校准**：高置信错误比普通错误更值得快速复测，且能识别“以为会了”。
6. **Feynman 解释转成新测项**：把含糊、跳步和类比失效变成下一次可验证的 item。
7. **状态有界、数据可读、可备份**：Markdown/JSON 优先；数据库只有在数据规模或并发真的需要时才合理。
8. **写入前验证**：学习卡片要对原始资料核验；长期画像与约束要区分事实、推断和待验证假设。

## 噱头或当前低价值功能

- **“循证方法”数量与效果量堆叠**：研究支持某种学习方法，不等于这个仓库实现已经被验证有效。
- **倦怠检测、学习风格、职业匹配**：少量行为数据不足以支持稳定推断；容易把猜测写进画像。
- **等级、徽章、streak、排行榜**：可提高参与感，但不能替代迁移题、延迟复测和开放解释。
- **强制学习契约与提醒检查**：对习惯养成可能有用，但不是 Learner State 的核心，还会增加每次会话成本。
- **大而全 Dashboard / OAuth / 多用户 /云部署**：对单用户 Codex 学习流是额外运维面。
- **自称“AI 个性化”**：若只是让模型自由改一段 profile 文本而没有证据、置信度和冲突处理，只是提示词包装。
- **知识图谱与复杂检索栈**：适合大型 Vault，不自动等于更准确的能力评估，也不必然更省 Token。

## 最终选择与使用策略

| 项目 | 明确建议 | 原因 |
|---|---|---|
| mastery-loop | **直接使用** | 唯一把 Codex Skill、主动回忆、Feynman、FSRS、错误驱动学习和本地 Markdown/JSON 放在同一条低运维路径上 |
| RetainCraft | **只借鉴设计** | 实现真实，但协议过宽；等级测试与提醒不是当前核心 |
| MindTrain | **只借鉴设计** | Codex 接入与权威数据边界优秀，但部署重、FSRS 未实现、开放回答弱 |
| SkillClimb | **只借鉴设计** | 评估能力强，但平台复杂度和内容建模成本过高 |
| 两个 Learning Loop | **只借鉴设计** | 它们管理 AI/知识库的学习，不评估人的学习 |

不建议现在 Fork。先直接使用 mastery-loop，让一次真实主题产生至少 10–20 次回答、若干错误、跨日复习和一次延迟复测；届时才有证据判断是否需要一个极小 Fork，可能只涉及：统一跨主题 learner profile、显式 misconception 记录、对抗式问题类型和更严格的状态写入证据字段。那仍应是对现有项目的窄修改，而不是新系统。

## 维护与许可证快照

| 仓库 | License | 默认分支/仓库活动快照（审计时） | 运行性判断 |
|---|---|---|---|
| all666666all/mastery-loop | MIT | 默认分支最后提交 2026-06-20；仓库新且小 | 本机 36/36 通过 |
| kaixiad/RetainCraft | MIT | 默认分支最后提交 2026-05-24 | 本机 170/170 通过 |
| shigella520/MindTrain | MIT | 默认分支最后提交 2026-08-27；公开 CI passing | 代码/CI 可信，本机未做 Docker E2E |
| danallison/skillclimb | MIT | 默认分支最后提交 2026-06-22，API 显示仓库 2026-07-24 有 push | 本机未取得完整测试通过证据 |
| melodykoh/learning-loop-skill | MIT | API 快照显示 2026-08-28 push | 文档/Skill 可读，未运行完整工作流 |
| robinslange/learning-loop | Apache-2.0 | API 快照显示 2026-08-31 push | 活跃但系统复杂，未做完整安装 |

“最近维护”只说明更新活跃度，不证明长期维护承诺；这些仓库大多在 2026 年创建，长期稳定性仍是 UNKNOWN。

## 证据限制

- 审计基于仓库 README、Skill、核心调度/数据库源码、测试入口、License、GitHub 元数据和有限本机测试；没有长期真实学习效果数据。
- mastery-loop 与 RetainCraft 的测试证明调度与数据操作满足其测试，不证明 AI 教学质量或用户长期收益。
- MindTrain 未在本机启动完整 Compose；SkillClimb 未在本机完成依赖与测试；这两项的“实际可运行程度”保留为部分验证，而不是完成声明。
- Star 数不纳入核心评分；项目新、样本小，受欢迎程度对本目标的证据价值很低。

## 主要一手来源

- [mastery-loop README](https://github.com/all666666all/mastery-loop)、[Skill](https://github.com/all666666all/mastery-loop/blob/main/SKILL.md)、[scheduler](https://github.com/all666666all/mastery-loop/blob/main/scripts/scheduler.py)、[FSRS](https://github.com/all666666all/mastery-loop/blob/main/scripts/fsrs.py)、[MIT License](https://github.com/all666666all/mastery-loop/blob/main/LICENSE)
- [MindTrain README](https://github.com/shigella520/MindTrain)、[Skill](https://github.com/shigella520/MindTrain/blob/main/plugins/mindtrain/skills/mindtrain/SKILL.md)、[schema](https://github.com/shigella520/MindTrain/blob/main/apps/trainer-core/src/main/resources/db/migration/V1__initial_schema.sql)、[weighted scheduler](https://github.com/shigella520/MindTrain/blob/main/apps/trainer-core/src/main/java/io/github/shigella520/mindtrain/core/scheduling/WeightedSchedulerProvider.java)、[MIT License](https://github.com/shigella520/MindTrain/blob/main/LICENSE)
- [SkillClimb README](https://github.com/danallison/skillclimb)、[SM-2 core](https://github.com/danallison/skillclimb/blob/main/packages/core/src/srs/sm2.ts)、[data export/import](https://github.com/danallison/skillclimb/blob/main/docs/data-export-import.md)、[MIT License](https://github.com/danallison/skillclimb/blob/main/LICENSE)
- [RetainCraft README](https://github.com/kaixiad/RetainCraft)、[Skill](https://github.com/kaixiad/RetainCraft/blob/main/SKILL.md)、[SRS engine](https://github.com/kaixiad/RetainCraft/blob/main/scripts/srs.py)、[tests](https://github.com/kaixiad/RetainCraft/blob/main/scripts/test_srs.py)、[MIT License](https://github.com/kaixiad/RetainCraft/blob/main/LICENSE)
- [learning-loop-skill README](https://github.com/melodykoh/learning-loop-skill)、[Skill](https://github.com/melodykoh/learning-loop-skill/blob/main/SKILL.md)、[MIT License](https://github.com/melodykoh/learning-loop-skill/blob/main/LICENSE)
- [robinslange/learning-loop README](https://github.com/robinslange/learning-loop)、[configuration](https://github.com/robinslange/learning-loop/blob/main/guide/configuration.md)、[cross-platform notes](https://github.com/robinslange/learning-loop/blob/main/guide/cross-platform.md)、[Apache-2.0 License](https://github.com/robinslange/learning-loop/blob/main/LICENSE)
