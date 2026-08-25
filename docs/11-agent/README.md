# 11-agent

> Agent（智能体）知识体系。覆盖 Agent 的定义与边界、运行架构、核心组件、如何驱动真实研发 Workflow，以及 Agent 工程化最佳实践与常见问题。

---

## 本章节内容

| 文档 | 内容 |
| --- | --- |
| `01-overview.md` | 概述：Agent 是什么、与 Chat AI 的演进关系、在 AI FullStack Workflow 中的位置、与 Model / Prompt / Rules / MCP / Tools 的边界、本项目实际 Agent 资产 |
| `02-agent-architecture.md` | Agent 架构：基本架构图、Agent Loop、决策 / 上下文 / 工具调用 / 状态 / 记忆 / 权限 / 错误恢复，以及与传统应用架构的区别 |
| `03-agent-components.md` | 核心组件：Model / Prompt / Rules / Context / Memory / Tools / MCP 及内部角色（Planner / Executor / Observer / Validator / HITL），组件如何协作、不同 Agent 的组合差异 |
| `04-workflow.md` | Workflow：从基础环到各实际 Agent（以 flutter-agent 为样板）的 Workflow，以及 Multi-Agent 与完整 AI FullStack Workflow 编排 |
| `05-best-practice.md` | 最佳实践：Agent 工程化的二十条原则——单一职责、边界、Context 控制、权限、HITL、成本、长期维护等 |
| `06-faq.md` | FAQ：35 个常见问题速查，覆盖概念、架构、组件、各实际 Agent 与企业落地 |

## 核心观点

- **Agent 不是聊天机器人，也不是 Model 本身**：它是把模型、上下文、规则、工具、决策与执行组织起来的工作机制——一个能"感知现状、规划、调用工具、验证"的执行层。
- **Agent 是前面章节的汇总与提升**：Prompt 提供意图、Rules 提供纪律、Model 提供推理、MCP 提供工具，Agent 把它们编排成自主执行的闭环。
- **Agent 的价值在于"资产化"**：一次配置（prompt.md + workflow.md）长期复用，团队每个成员获得一致的协作体验（`agents/` 目录即是资产库）。
- **可控性是第一原则**：Human-in-the-loop、权限边界、错误恢复不是附加项，而是 Agent 能否落地的决定因素。

## 与其他章节的关系

```text
docs/02-models/   推理能力层 —— Agent 依赖 Model 做推理与决策
docs/09-rules/    行为约束层 —— Rules 是 Agent 长期遵守的纪律
docs/10-prompts/  任务意图层 —— Prompt 是每次委派给 Agent 的任务说明
docs/11-agent/    执行层     —— 本章节，把上述能力组织成自主执行闭环
docs/08-mcp/      工具能力层 —— MCP 为 Agent 提供触达外部工具与数据的通道
```

阅读顺序建议：先读 `02-models/01-ai-model-overview.md` 理解推理层，再读 `09-rules`、`10-prompts` 理解约束层与意图层，最后回到本章理解执行层如何把它们统一编排。

## 阅读建议

- **想快速理解 Agent 是什么**：读 `01-overview.md`。
- **想理解 Agent 怎么运转**：读 `02-agent-architecture.md` 与 `03-agent-components.md`。
- **想看实际工作流**：读 `04-workflow.md`，重点对照 `agents/flutter-agent/` 的真实资产。
- **想在团队落地 Agent**：直接看 `05-best-practice.md`。
- **遇到问题**：查 `06-faq.md`。

> 注：本章节是 Agent 的**知识体系与方法论**；实际可复用的 Agent 配置见 `agents/` 目录。两者职责分离——文档讲解原理与设计，资产提供可直接加载的系统提示词与流程定义，不重复复制正文。
