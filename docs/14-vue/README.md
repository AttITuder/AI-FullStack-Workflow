# 14-vue

> Vue AI 工程体系。本文档不是 Vue 入门教程，而是讲解"如何在 AI FullStack Workflow 中工程化地组织 Vue 前端开发"——如何用 Model、Prompt、Rules、Agent、MCP 提升 Vue 项目的开发效率。

---

## 本章节内容

| 文档 | 内容 |
| --- | --- |
| `01-overview.md` | Vue AI 开发概述：Vue 在 AI FullStack Workflow 中的位置、技术生态、前端工程特点、与 Prompt / Rules / Agent / MCP 的关系、Vue AI Workflow 与学习路线 |
| `02-architecture.md` | Vue 工程架构：整体架构、页面层、状态管理、路由层、数据层、组件设计、前端工程化、大型项目架构、AI 辅助架构分析、Agent 如何理解 Vue 项目 |
| `03-development-workflow.md` | Vue 开发 Workflow：新页面、组件、API 接入、状态管理、UI 还原、Bug 修复、性能优化、重构、Code Review、Prompt + Rules + Agent 的组合流程 |
| `04-ai-assisted-development.md` | AI 辅助 Vue 开发：AI 辅助的场景、理解 Vue 项目、架构设计、问题定位、Prompt / Rules / MCP 的配合，以及企业级落地 |
| `05-best-practice.md` | Vue AI 开发最佳实践：Context 管理、Prompt 设计、Rules 管理、Agent 使用、MCP 使用、组件设计、状态管理、性能优化、TypeScript 规范、Code Review、测试、发布 |
| `06-faq.md` | FAQ：常见问题速查，覆盖 Vue3 / Composition API / TypeScript / Vite / Pinia / Router / 组件化 / 工程化 / AI 生成 / Prompt / Rules / Agent / MCP / Debug / 性能 / 测试 / 企业项目 |

## Vue 在 AI FullStack Workflow 中的位置

Vue 是 AI FullStack Workflow 的全栈实战层之一（`docs/14-vue/`），与 Flutter、Android 同属全栈实战，位于基础认知与工具实操之后、企业研发之前：

```text
docs/01-ai-basic/  认知层
docs/02-models/    推理能力层
docs/03~07/        工具实操层（OpenCode / Codex / Claude Code / CodeBuddy / Pi）
docs/08-mcp/       工具能力层（MCP 协议）
docs/09-rules/     行为约束层（Rules 库）
docs/10-prompts/   任务意图层（Prompt 库）
docs/11-agent/     执行层（Agent 体系）
docs/12-flutter/   全栈实战层（Flutter）
docs/13-android/   全栈实战层（Android）
docs/14-vue/       全栈实战层（本章节 Vue 工程体系）
docs/15-enterprise/ 企业研发层
```

Vue 章节把前面沉淀的 **Model + Prompt + Rules + Agent + MCP** 能力，落到 Vue 这个组件化、响应式、工程化成熟的前端技术栈上。它是前面所有方法论在 Vue 场景下的**工程化集成示范**。

## Vue 与 Prompt 的关系

- `prompts/vue/` 用于沉淀 Vue 专属 Prompt 模板（当前待补充），规划覆盖页面生成、组件生成、UI 还原、接口联调、状态管理等。
- 章节讲解如何编写高质量 Vue 开发 Prompt、如何用 `【】` 占位符复用模板、如何把需求翻译成可执行任务（见 `04-ai-assisted-development.md`、`05-best-practice.md`）。

## Vue 与 Rules 的关系

- `rules/vue/` 用于沉淀 Vue 编码规范（当前待补充），约束 AI 与开发者产出风格一致（Vue3 / Composition API / TypeScript / 组件 / 命名 / 分层）。
- 章节讲解 Rules 如何约束 Vue 代码、如何让 Agent 遵守规则以及如何作为 Review 依据（见 `05-best-practice.md`）。

## Vue 与 Agent 的关系

- Vue 前端开发由前端 / 通用 Agent 支撑（项目中 `agents/` 沉淀了 `architect-agent`、`review-agent`、`test-agent` 等流程与架构 Agent，可在 Vue 项目复用）。
- 章节讲解 Agent 如何理解 Vue 项目（组件树、路由、状态、工程化配置），如何在真实工程里驱动页面与组件生成、定位问题与审查（见 `02-architecture.md`、`03-development-workflow.md`）。
- 文档讲原理与流程，Agent 资产提供可加载的配置，三者职责分离，不重复复制。

## Vue 学习路线

按以下顺序阅读：

1. **建立整体认知**：读 `01-overview.md`，理解 Vue 在 AI FullStack Workflow 中的位置与技术生态。
2. **理解工程结构**：读 `02-architecture.md`，掌握 Vue 各层职责与可选的架构方案。
3. **掌握开发流程**：读 `03-development-workflow.md`，跑通需求 → Prompt → Rules → Agent → Vue 代码 → 测试 → Review。
4. **学会用 AI 提效**：读 `04-ai-assisted-development.md`，看 AI 如何在页面、组件、UI、状态等场景辅助。
5. **沉淀团队实践**：读 `05-best-practice.md`，落地为团队规范。
6. **遇到问题查漏**：读 `06-faq.md`。

## 阅读建议

- **想快速了解 Vue 章节**：读 `01-overview.md`。
- **想理解 Vue 工程结构**：读 `02-architecture.md`。
- **想跑通一整套 Vue 研发流程**：读 `03-development-workflow.md`。
- **想用 AI 提升 Vue 开发效率**：读 `04-ai-assisted-development.md`。
- **想在团队落地最佳实践**：读 `05-best-practice.md`。
- **遇到具体问题**：查 `06-faq.md`。

> 注：本章节是 **Vue AI 工程体系的知识与方法论**；实际可复用的 Prompt 与 Rules 分别见 `prompts/vue/`、`rules/vue/`，Agent 配置见 `agents/`。职责分离——文档讲解如何组织 Vue + AI 工作流，资产提供可直接复用的模板与配置。
