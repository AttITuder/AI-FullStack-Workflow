# 13-android

> Android AI 工程体系。本文档不是 Android 入门教程，而是讲解"如何在 AI FullStack Workflow 中工程化地组织 Android 开发"——如何用 Model、Prompt、Rules、Agent、MCP 提升 Android 项目的开发效率。

---

## 本章节内容

| 文档 | 内容 |
| --- | --- |
| `01-overview.md` | Android AI 开发概述：Android 在 AI FullStack Workflow 中的位置、工程开发特点、复杂度来源、与 Agent / Prompt / Rules / MCP 的关系、Android AI Workflow 与学习路线 |
| `02-architecture.md` | Android 工程架构：整体架构、UI 层、状态管理、Domain / Data 层、Repository、网络层、本地数据、多模块、大型项目架构、AI 辅助架构分析、Agent 如何理解项目结构 |
| `03-development-workflow.md` | Android 开发 Workflow：新功能、页面、API、数据库、三方 SDK、Bug 修复、Crash、性能优化、重构、Code Review、Android Agent、Prompt + Rules + Agent 的组合流程 |
| `04-ai-assisted-development.md` | AI 辅助 Android 开发：AI 辅助的场景、理解代码、架构设计、问题定位、Agent / MCP / Rules / Prompt 的配合，以及企业级落地 |
| `05-best-practice.md` | Android AI 开发最佳实践：上下文管理、Prompt 设计、Rules 管理、Agent 使用、MCP 使用、Kotlin / Java / Compose / XML、Code Review、测试、发布、安全 |
| `06-faq.md` | FAQ：常见问题速查，覆盖 Android / Kotlin / Java / Compose / XML / Gradle / 架构 / MVVM / 多模块 / AI 生成 / Prompt / Rules / Agent / MCP / Debug / Crash / 性能 / 测试 / 发布 / 兼容性 / 企业项目 |

## Android 在 AI FullStack Workflow 中的位置

Android 是 AI FullStack Workflow 的全栈实战层之一（`docs/13-android/`），与 Flutter、Vue 并列，位于基础认知与工具实操之后、企业研发之前：

```text
docs/01-ai-basic/  认知层
docs/02-models/    推理能力层
docs/03~07/        工具实操层（OpenCode / Codex / Claude Code / CodeBuddy / Pi）
docs/08-mcp/       工具能力层（MCP 协议）
docs/09-rules/     行为约束层（Rules 库）
docs/10-prompts/   任务意图层（Prompt 库）
docs/11-agent/     执行层（Agent 体系）
docs/12-flutter/   全栈实战层（Flutter）
docs/13-android/   全栈实战层（本章节 Android 工程体系）
docs/14-vue/       全栈实战层（Vue）
```

Android 章节把前面沉淀的 **Model + Prompt + Rules + Agent + MCP** 能力，落到 Android 这个体系复杂、适配面广、工程链路长的技术栈上。它是前面所有方法论在 Android 场景下的**工程化集成示范**。

## Android 与 Agent 的关系

- `agents/android-agent/` 沉淀了 Android 专属 Agent（当前配置待丰富），负责需求分析、页面生成、Bug 定位、性能与架构辅助。
- 章节讲解 Agent 如何理解 Android 项目结构（Gradle、模块、Activity / Fragment / Compose、ViewModel 等），如何在真实工程里驱动开发与定位问题（见 `02-architecture.md`、`03-development-workflow.md`）。
- 文档讲原理与流程，Agent 资产提供可直接加载的配置，两者职责分离，不重复复制。

## Android 与 Prompt 的关系

- `prompts/android/` 规划沉淀 Android 专属 Prompt 模板：页面生成、Jetpack Compose、接口联调、Bug 修复、代码 Review。
- 章节讲解如何编写高质量 Android 开发 Prompt、如何用 `【】` 占位符复用模板、如何把需求翻译成可执行任务（见 `04-ai-assisted-development.md`、`05-best-practice.md`）。

## Android 与 Rules 的关系

- `rules/android/` 用于沉淀 Android 编码规范，约束 AI 与开发者产出风格一致（Kotlin / Java / 命名 / 分层 / 架构约束）。
- 章节讲解 Rules 如何约束 Android 代码、如何让 Agent 遵守规则以及如何作为 Review 依据（见 `05-best-practice.md`）。

## Android 学习路线

按以下顺序阅读：

1. **建立整体认知**：读 `01-overview.md`，理解 Android 在 AI FullStack Workflow 中的位置与复杂度来源。
2. **理解工程结构**：读 `02-architecture.md`，掌握 Android 各层职责与可选的架构方案。
3. **掌握开发流程**：读 `03-development-workflow.md`，跑通需求 → Prompt → Rules → Agent → 代码 → Build → Test → Review。
4. **学会用 AI 提效**：读 `04-ai-assisted-development.md`，看 AI 如何在 Kodlin / Compose / Crash / 性能等场景辅助。
5. **沉淀团队实践**：读 `05-best-practice.md`，落地为团队规范。
6. **遇到问题查漏**：读 `06-faq.md`。

## 阅读建议

- **想快速了解 Android 章节**：读 `01-overview.md`。
- **想理解 Android 工程结构**：读 `02-architecture.md`。
- **想跑通一整套 Android 研发流程**：读 `03-development-workflow.md`。
- **想用 AI 提升 Android 开发效率**：读 `04-ai-assisted-development.md`。
- **想在团队落地最佳实践**：读 `05-best-practice.md`。
- **遇到具体问题**：查 `06-faq.md`。

> 注：本章节是 **Android AI 工程体系的知识与方法论**；实际可复用的 Prompt 与 Agent 配置分别见 `prompts/android/`、`agents/android-agent/`。三者职责分离——文档讲解如何组织 Android + AI 工作流，资产提供可直接复用的模板与配置。
