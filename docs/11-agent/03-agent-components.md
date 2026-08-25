# Agent 核心组件

> 本章节拆解 Agent 的内部组件。Agent 并非铁板一块——它由若干可独立设计的部件组成，不同 Agent 采用不同组合。理解组件与协作方式，是设计自有 Agent 的基础。

---

## 组件总览

一个 Agent 可能包含以下组件（并非全部强制）：

```text
Agent
├── Model        推理引擎
├── Prompt       任务意图（系统提示词 + 本次任务）
├── Rules        行为约束
├── Context      当前上下文
├── Memory       长期记忆
├── Tools        可执行动作
├── MCP          工具通道
├── Planner      规划器
├── Executor     执行器
├── Observer     观察者
└── Validator    验证器
```

外加贯穿全程的：

```text
Human-in-the-loop（人在环中）
```

下面逐一说明，并解释它们如何协作。

## 1. Model

Agent 的推理引擎。负责理解意图、做规划、生成内容、判断结果。详见 `docs/02-models/01-ai-model-overview.md`。

设计关注点：

- **能力匹配**：Coding / Reasoning 强的模型更适合复杂开发 Agent；轻量任务可用小模型降本。
- **上下文窗口**：长代码库任务需要足够大的窗口或检索策略。
- **Tool Calling / MCP 支持**：Agent 化能力的基石，缺之则 Agent 无法动手。

## 2. Prompt

Agent 的"任务委托书"，分两层（见 `docs/10-prompts/`）：

- **系统提示词**：角色、职责、技术栈约束、工作原则，写入 `agents/<name>/prompt.md`，长期生效。
- **任务提示词**：本次委派的具体指令，单次生效。

在组件中，Prompt 是 Agent 行为的"初始程序"——它定义了 Agent "是谁、该干什么、怎么干"。

## 3. Rules

Agent 长期遵守的纪律，来自 `rules/`。组件职责是"约束"，与 Prompt 的区别：Rules 对所有任务成立、可跨 Agent 复用；Prompt 偏本次或本角色。Agent 引用 Rules 而非内联重复（见 `docs/09-rules/`）。

## 4. Context

本次任务中 Agent 能"看到"的信息总和：代码库、会话历史、相关文档。Context 组件负责**收集与维护**这些信息。管理原则见 `02-agent-architecture.md` 第 4 节：按需加载、控制窗口、阶段归档。

## 5. Memory

跨会话的长期记忆。在工程上由三类机制承担（见 `02-agent-architecture.md` 第 7 节）：

- 稳定偏好 → `rules/` 或 `prompt.md`。
- 可复用做法 → `prompts/` 或 `workflow.md`。
- 一次性经验 → 会话归档，复盘后择优上升。

Memory 组件让 Agent "越用越懂你 / 越用越懂项目"。

## 6. Tools

Agent 可执行的**具体动作单元**：读文件、写文件、运行命令、调用 API。Tool 必须"有界且可验证"——动作明确、结果可观察。Tools 可由 Agent 宿主原生提供，也可经 MCP 提供（下一节）。

## 7. MCP

Tools 的**标准化来源协议**。MCP Server 暴露一组 Tools 与 Resources，Agent 通过它连接 GitHub、数据库、设计稿、CI 等外部系统。详见 `docs/08-mcp/`。在组件视角，MCP 是 Tools 的"供给层"——它决定了 Agent 能力的外延。

## 8. Planner

规划器，负责把目标拆解为执行计划（步骤序列）。它是 Agent Loop 中 Reasoning→Planning 段的承担者。

工程体现：

- flutter-agent 的 `workflow.md` 要求"先分析、再设计、后生成"——这就是 Planner 在流程中的显式落地。
- 复杂任务可要求 Planner 先输出方案**待确认**，吸收不确定性（见 `docs/10-prompts/03-prompt-patterns.md` Step-by-Step 模式）。

## 9. Executor

执行器，负责把计划变成动作——调用具体 Tools 改变环境（写代码、跑命令）。它是 Agent Loop 中 Tool Calling 段的承担者。

设计要点：

- 严格按 Planner 的计划执行，不擅自扩大范围。
- 每步动作可回滚，便于错误恢复（见 `02-agent-architecture.md` 第 9 节）。
- 受权限边界约束（文件 / 命令 / 环境 / 数据）。

## 10. Observer

观察者，负责在每步动作后**收集结果**并判断"是否按预期"。它是 Agent Loop 中 Observation 段的承担者，也是 Agent 区别于死脚本的关键——脚本不观察，Agent 观察后决策。

Observer 的输出喂给 Validator 与下一步决策：成功则继续，异常则触发错误恢复。

## 11. Validator

验证器，负责对照**验收标准**判断任务是否真正完成。它把"AI 说完成了"变成"客观核对通过"。

工程体现：

- flutter-agent 的 `workflow.md` 自检环节即 Validator：`[ ] 代码可编译运行` `[ ] 遵循 Clean Architecture` 等清单。
- 验收清单来自真实踩坑（见 `docs/10-prompts/05-best-practice.md`），每条背后应有一个它防住的失败。

Validator 既用于 AI 自检，也用于人工最终验收——同一份清单双重使用。

## 12. Human-in-the-loop

人在环中（HITL），贯穿所有组件的"元组件"。它不是某个阶段，而是一种设计态度：在关键节点让人的判断介入。

典型介入点：

- **目标级**：任务模糊时确认意图（对应 Planner 入口）。
- **方案级**：执行前确认计划（吸收最大不确定性）。
- **风险级**：危险操作（删除、推送、部署）前确认。
- **验收级**：最终交付前人工 Review。

HITL 不是"Agent 不够聪明"的补丁，而是**可控性的根本保证**——见 `05-best-practice.md` 第 9 节。

## 组件如何协作

把组件代入 Agent Loop，得到完整协作链：

```text
Prompt + Rules ──→ Planner（规划）
                       ↓
                   Executor（调用 Tools / MCP）
                       ↓
                   Observer（观察结果）
                       ↓
                   Validator（对照验收）
                       ↓
                 Next Action（继续 / 修正 / 完成）
                       ↓
                   Human-in-the-loop（关键节点介入）
```

Model 贯穿全程提供推理；Context / Memory 在每一步为 Planner 与 Validator 提供信息；MCP 是 Executor 的能力供给。

## 不同 Agent 采用不同组合

**不要以为每个 Agent 都必须具备全部组件。** 组件是工具箱，按需取用：

| Agent 类型 | 典型组件组合 | 说明 |
| --- | --- | --- |
| 轻量对话 Agent | Model + Prompt + Context | 无需 Tools，只答疑 |
| 编码 Agent | 全组件（含 Tools/MCP/Validator） | 需改环境、需验证 |
| 审查 Agent | Model + Prompt + Rules + Observer/Validator | 只读、给意见，不写代码 |
| 分析 Agent | Model + Prompt + Context + Planner | 产出分析报告，动作少 |

### 结合本项目 `agents/`

- **flutter-agent**（已落地）：组件最完整——`prompt.md` 提供 Model + Prompt + Rules，`workflow.md` 定义 Planner / Executor / Observer / Validator 的衔接（需求→分析→设计→生成→自检→交付），并通过 MCP / Tools 读写代码与构建。是当前"全组件 Agent"的样板。
- **review-agent / test-agent / product-agent**（规划中）：预期是"轻执行"组合——review 偏 Observer+Validator（只读给意见），product 偏 Planner（需求拆解），test 偏 Executor+Validator（跑测试）。具体以 `agents/` 后续落地为准。
- **android / architect / git / release**（规划中）：组件组合待定，遵循同样的"按职责选组件"原则。

> 设计自有 Agent 时，先问"它需要感知什么、改变什么、验证什么"，再决定装配哪些组件——过度装配只会增加成本与风险。

## 总结

- Agent 由 Model / Prompt / Rules / Context / Memory / Tools / MCP / Planner / Executor / Observer / Validator 十一类组件 + HITL 组成。
- Model 提供推理，Prompt 给意图，Rules 给纪律，Context/Memory 供信息，Tools/MCP 供动作，Planner/Executor/Observer/Validator 构成执行闭环。
- HITL 是贯穿全程的元组件，保证可控性。
- 组件协作链：Prompt+Rules→Planner→Executor→Observer→Validator→Next Action。
- **不同 Agent 不同组合**：轻量 Agent 可只有 Model+Prompt，编码 Agent 才需全组件。
- 结合 `agents/`：flutter-agent 是"全组件"样板，其余 Agent 按需装配，不强行统一。

## 参考资料

- 上一节：`02-agent-architecture.md`
- 下一节：`04-workflow.md`
- 意图层：`docs/10-prompts/03-prompt-patterns.md`
- 实际资产：`agents/flutter-agent/`
