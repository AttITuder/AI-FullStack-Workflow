# Agent 概述

> 本文回答一个根本问题：Agent 是什么，为什么在 AI Coding 中不可或缺，以及它在 AI FullStack Workflow 六层架构中处于什么位置。

---

## 1. Agent 是什么

### AI Agent

AI Agent（智能体）是一种能够**自主感知环境、进行推理规划、调用工具并采取行动**以完成目标的 AI 系统。它区别于"只回答问题的模型"：Agent 不止输出文本，还会读写文件、执行命令、查询数据，并在行动后观察结果、决定下一步。

### Coding Agent

Coding Agent 是面向软件研发的 Agent 形态。它以代码库为环境，以"完成工程任务"为目标——生成代码、修复 Bug、运行测试、提交变更。前面章节介绍的 OpenCode、Codex、Claude Code、CodeBuddy、Pi Agent（见 `docs/03-opencode/` 到 `docs/07-pi-agent/`）都是 Coding Agent 工具的不同实现。

### Autonomous Agent

Autonomous Agent 强调"自主性"：在给定目标后，能自行拆解步骤、连续调用工具、自我纠正，直至目标达成，期间尽量少依赖人工逐步指令。自主程度是光谱而非开关——多数工程 Agent 在"方案确认""最终 Review"等关键点仍需人介入（见 `05-best-practice.md` Human-in-the-loop）。

### Agent 与普通 Chat AI 的区别

| 维度 | Chat AI | Agent |
| --- | --- | --- |
| 产出形式 | 文本回答 | 文本 + 环境动作（改文件、跑命令） |
| 任务跨度 | 单轮问答 | 多步自主执行 |
| 环境交互 | 无 | 读代码库、调工具、看结果 |
| 目标达成 | 提供信息 | 直接交付结果 |
| 失败处理 | 重新提问 | 观察→纠正→重试 |

一句话：**Chat AI 告诉你怎么做，Agent 替你做。**

## 2. 为什么需要 Agent

从开发者使用 AI 的方式看，是一条清晰的演进链：

```text
Chat AI            问答式辅助，复制答案自己改
   ↓
Copilot           行内补全，AI 写下一行
   ↓
Coding Agent      自主执行多步任务，改文件、跑命令
   ↓
Agent Workflow    多个 Agent + 规则 + 工具组成稳定可复现的研发流程
```

演进的驱动力是**把"人的注意力"从每一步操作中解放出来**：

- Chat AI 阶段：人要读懂答案、自己落地。
- Copilot 阶段：人仍掌控每个函数怎么写。
- Coding Agent 阶段：人给目标，Agent 拆解并执行，人只做关键确认。
- Agent Workflow 阶段：整条研发链路被标准化、可复用、可团队协作。

本项目（`docs/00-roadmap/project-positioning.md`）指出，2024 年起 Agent 让 AI 从"回答问题"进化到"自主执行端到端工程工作"。Agent 章节正是这一演进的抽象与沉淀。

## 3. Agent 的基本工作方式

Agent 的运行是一个持续循环的闭环：

```text
Observe（观察现状）
   ↓
Think（推理分析）
   ↓
Plan（制定计划）
   ↓
Act（执行动作 / 调用工具）
   ↓
Observe（观察结果）
   ↓
（回到 Think，直至目标达成）
```

这个循环有两个关键特征：

1. **观察驱动**：每一步行动后都会重新观察环境，而不是盲目按初始计划一路执行——这让它具备自我纠正能力。
2. **终止条件**：循环何时停？通常是"目标达成"或"达到验证标准"，也可能是"需要人工决策"或"遇到不可恢复的错误"。

> 注意：本项目在 `agents/flutter-agent/workflow.md` 中把这一抽象循环具体化为工程流程 `需求 → 分析 → 设计 → 生成 → 自检 → 交付`——前者是通用工作方式，后者是可落地的工程实例化。

## 4. Agent 在 AI FullStack Workflow 中的位置

把 Agent 放进六层架构（承接 `docs/02-models/`、`docs/09-rules/`、`docs/10-prompts/`、`docs/08-mcp/`）：

```text
Model        推理能力层   "大脑"：理解、推理、生成
   ↓
Prompt       任务意图层   "委托书"：这次要做什么
   ↓
Rules        行为约束层   "施工规范"：长期应该怎么做
   ↓
Agent        执行层       "项目经理"：把以上组织成自主执行闭环
   ↓
MCP          工具能力层   "设备通道"：触达外部工具与数据
   ↓
Tools        具体能力     MCP 暴露的具体函数（构建、查询、部署）
   ↓
Execution    执行动作     读写文件、跑命令、改代码
   ↓
Validation   验证闭环     对照验收标准核对产出
```

Agent 处于承接上下位置：**向下**消费 Model 的推理、Prompt 的意图、Rules 的纪律；**向上**驱动 MCP / Tools 实际改变环境；**自身**负责把"意图"翻译成"行动序列"并对结果负责。

最终端到端链路（本章 `04-workflow.md` 详述）：

```text
需求 → Prompt → Rules → Agent → Model → MCP / Tools → 执行 → 验证 → Review
```

## 5. Agent 与 Coding Agent 的关系

Coding Agent 是 Agent 在软件研发领域的**一种具体形态**，不是新概念。Agent 的通用定义（感知—推理—行动）在所有领域都成立；当"环境"是代码库、"动作"是编码与命令、"目标"是工程任务时，它就是 Coding Agent。

理解这点很重要：本章讲的方法（架构、组件、循环、工作流）对所有 Agent 通用；`agents/` 下各技术栈 / 流程 Agent 是这些方法在具体研发场景的实例化。不要因为"只会写代码"就低估 Agent 的通用性——同一套机制适用于产品分析、架构设计、发布等非编码任务。

## 6. Agent 与 Model 的关系

**Agent 不等于 Model，Model 是 Agent 的推理引擎。**

- 没有 Model，Agent 无法理解意图、无法规划——它退化成一个死脚本。
- 没有 Agent，Model 只是"会聊天的文本生成器"——它不会主动读写你的代码库、不会连续行动。

关键边界（`docs/02-models/01-ai-model-overview.md`）：

- Model 本身只处理文本，输出概率生成的 Token，受上下文窗口限制。
- Agent 通过 **Tool Calling** 与 **MCP 支持** 让 Model "动手"——这两项是 Agent 化的基石能力。
- Model 决定"能力上限"，Agent 决定"能力是否被有效组织与运用"。

类比：Model 是施工队的技术水平，Agent 是项目经理——技术再强，没有组织也只是一盘散沙。

## 7. Agent 与 Prompt 的关系

**Prompt 是交给 Agent 的"任务委托书"，Agent 是 Prompt 的宿主与执行者。**

两层 Prompt：

1. **系统提示词（长期）**：定义 Agent 的角色、职责、技术栈约束、工作原则——对应 `agents/flutter-agent/prompt.md`。它在所有任务中都生效，性质接近 Rules。
2. **任务 Prompt（单次）**：每次委派的具体指令——"生成登录页""修复这个崩溃"。详见 `docs/10-prompts/`。

关系要点：

- 简单任务：直接对话，任务 Prompt 即全部输入。
- 高频任务：把成熟任务 Prompt 沉淀进 `prompts/` 或升级为 Agent 配置的一部分。
- Agent 的工作流（workflow.md）往往规定了"如何拆解任务 Prompt"，例如先分析再生成。

## 8. Agent 与 Rules 的关系

**Rules 是 Agent 长期遵守的纪律，Agent 是 Rules 的执行者。**

一句话区分（呼应 `docs/09-rules/01-overview.md`）：

> Prompt 管一次任务，Rules 管长期行为，Agent 在两者约束下执行。

分工：

- Rules 写在 `rules/` 目录，对所有任务生效（编码规范、审查规范、提交规范）。
- Agent 的 `prompt.md` 通常引用对应技术栈的 Rules，例如 flutter-agent 要求"遵循项目中 `rules/` 下的编码规范"。
- Agent 自己不必重复实现规则——引用即可，规则更新时 Agent 自动受益。

工程含义：当某条约束反复出现在多个 Agent / Prompt 中，就该下沉为 Rules 长期生效。

## 9. Agent 与 MCP 的关系

**MCP 为 Agent 提供"手脚"——触达外部工具与数据的标准通道。**

- Agent 决定"要做什么动作"（如构建并安装到设备）。
- MCP 提供"动作怎么实现"（如 ADB MCP 执行安装、Gradle MCP 执行构建）。
- 没有 MCP，Agent 只能操作本地文件与命令行；有了 MCP，它能连接 GitHub、数据库、设计稿、CI 等真实研发环境（见 `docs/08-mcp/`）。

边界：写 Prompt / workflow 时假设工具可用即可，不必描述工具细节；工具调用的安全纪律由 Rules 约束（如"生产环境只读"）。

## 10. Agent 与 Tools 的关系

Tools 是 Agent 能执行的**具体动作单元**——读文件、写文件、运行命令、调用 API。MCP 是 Tools 的**协议与来源**之一（MCP Server 暴露一组 Tools）。

二者的关系：

```text
Agent
   └── 调用 Tool（动作单元）
         └── Tool 由 MCP Server 提供（也可能由 Agent 宿主原生提供）
```

设计含义：Agent 的能力上限 = 它可访问的 Tools 集合。给 Agent 更多、更安全的 Tools，它就能完成更复杂的任务；但 Tools 越多，权限与风险控制越重要（见 `05-best-practice.md` 权限控制）。

## 11. 本项目中的 Agent

本项目在 `agents/` 目录维护实际 Agent 资产。按当前建设状态分两类：

### 已落地的完整资产

- **Flutter Agent**（`agents/flutter-agent/`）：含 `README.md` + `prompt.md` + `workflow.md`。定位"负责 Flutter 开发"，能力覆盖需求分析、页面生成、Bug 修复、Code Review、重构；工作流为 `需求 → 分析 → 设计 → 生成 → 自检 → 交付`。是当前唯一可直接加载运行的完整 Agent 范例。

### 规划中（资产待补充）

以下 Agent 目录已在 `agents/` 建立，但 `prompt.md` / `workflow.md` 尚在建设中（README 标注"配置待补充"）：

- **Android Agent**：Android 开发 Agent。
- **Architect Agent**：架构设计 Agent。
- **Git Agent**：Git 提交与分支协作 Agent。
- **Product Agent**：产品需求分析 Agent。
- **Release Agent**：发布流程 Agent。
- **Review Agent**：代码审查 Agent。
- **Test Agent**：测试 Agent。

> 注意：本章节介绍这些 Agent 的**职责与设计方法**，不虚构其内部实现。具体配置以 `agents/` 各目录的实际文件为准；占位目录将在后续 Sprint 中逐步填充。

## 12. Agent 的适用场景

Agent 适合"目标清晰、步骤多、需要操作环境"的任务：

| 场景 | 说明 | 对应本项目 Agent |
| --- | --- | --- |
| 软件开发 | 新功能、页面生成、模块开发 | flutter-agent / android-agent |
| 项目分析 | 接手陌生代码库、技术债盘点 | ——（可由通用 Agent 承担） |
| 架构设计 | 分层设计、技术选型 | architect-agent |
| 测试 | 生成用例、跑测试、覆盖率 | test-agent |
| Code Review | 结构化审查意见 | review-agent |
| 发布 | 构建、打包、提审、上线 | release-agent |
| 产品分析 | 需求拆解、验收标准制定 | product-agent |

判断"该不该用 Agent"：纯问答用 Chat AI 即可；需要连续改文件 / 跑命令 / 多步完成的，才值得启动 Agent。

## 13. Agent 的局限性

Agent 不是银弹，落地前必须正视其局限：

- **错误决策**：模型会推理出错或误解意图，自主执行可能放大错误。对策：HITL 关卡 + 验证闭环。
- **上下文问题**：受上下文窗口限制，长任务后期可能"忘记"早期约束或误读现状。对策：Context 控制、阶段归档（见 `04-workflow.md`）。
- **工具调用风险**：错误调用工具可能删文件、推错分支、触达生产环境。对策：权限边界、危险操作人工确认。
- **权限问题**：Agent 拥有的权限越大，潜在破坏面越大。最小权限原则是底线。
- **成本**：多步自主执行 + 长上下文 + 反复重试，Token 消耗远高于单次问答。对策：成本控制策略（见 `05-best-practice.md`）。
- **可控性**：过度自主可能导致"它做了什么说不清"。对策：日志、变更清单、可回放。

> 这些局限不是"不用 Agent"的理由，而是"必须把 Agent 当工程系统来设计"的理由——本章 `02`~`05` 节都在回答如何控制这些风险。

## 14. 本章节学习路线

围绕"从概念到落地"：

1. **概述（本篇）**：定义、演进、位置、边界、局限。
2. **架构**：Agent 怎么运转（`02-agent-architecture.md`）。
3. **组件**：由哪些部件组成、如何协作（`03-agent-components.md`）。
4. **Workflow**：如何驱动真实研发流程，含各 Agent 实例（`04-workflow.md`）。
5. **最佳实践**：团队工程化落地原则（`05-best-practice.md`）。
6. **FAQ**：常见问题与排查（`06-faq.md`）。

配套阅读：

- 实际资产：`agents/flutter-agent/`（`prompt.md` + `workflow.md`）。
- 下层基础：`docs/02-models/01-ai-model-overview.md`（推理层）、`docs/09-rules/01-overview.md`（约束层）、`docs/10-prompts/01-overview.md`（意图层）、`docs/08-mcp/01-overview.md`（工具层）。

## 总结

- Agent 是自主感知、推理、行动、验证的执行机制，不是聊天机器人，也不等于 Model。
- 演进链：Chat AI → Copilot → Coding Agent → Agent Workflow，本质是把人的注意力从逐步操作里解放出来。
- 基本工作方式：Observe → Think → Plan → Act → Observe 的闭环，目标是达成目标或验证标准。
- 六层位置：Agent 在执行层，消费 Model / Prompt / Rules，驱动 MCP / Tools，对结果负责。
- 与相邻层：Model 是引擎、Prompt 是委托书、Rules 是纪律、MCP 是手脚、Tools 是动作单元。
- 本项目 Agent：`flutter-agent` 已完整落地，其余七个在建设中；文档不虚构其内部实现。
- 局限可控：错误决策、上下文、工具风险、权限、成本、可控性——都靠工程化手段管理。

## 参考资料

- 下一节：`02-agent-architecture.md`
- 推理层：`docs/02-models/01-ai-model-overview.md`
- 约束层：`docs/09-rules/01-overview.md`
- 意图层：`docs/10-prompts/01-overview.md`
- 工具层：`docs/08-mcp/01-overview.md`
- 实际资产：`agents/flutter-agent/`
