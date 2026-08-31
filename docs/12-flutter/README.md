# 12-flutter

> Flutter AI 工程体系。本文档不是 Flutter 教程，而是讲解"如何在 AI FullStack Workflow 中工程化地组织 Flutter 开发"——如何用 Model、Prompt、Rules、Agent、MCP 提升 Flutter 项目的开发效率。

---

## 本章节内容

| 文档 | 内容 |
| --- | --- |
| `01-overview.md` | Flutter AI 开发概述：Flutter 在 AI FullStack Workflow 中的位置、为何适合 AI 辅助、技术体系、与 Agent / Prompt / Rules 的关系、Flutter Workflow 与学习路线 |
| `02-architecture.md` | Flutter 工程架构：页面层、状态管理层、数据层、网络层、模块化、Package 管理、大型项目架构、Agent 如何理解 Flutter 架构 |
| `03-development-workflow.md` | Flutter 开发 Workflow：新功能、页面、API 接入、状态管理、Bug 修复、性能优化、重构、Code Review、Flutter Agent、Prompt + Rules + Agent 的组合流程 |
| `04-ai-assisted-development.md` | AI 辅助 Flutter 开发：AI 分析的场景、生成页面、优化代码、架构设计、Agent / MCP / Rules / Prompt 的配合，以及企业级落地 |
| `05-best-practice.md` | Flutter AI 开发最佳实践：项目分析、Context 管理、Prompt 编写、Rules 管理、Agent 使用、MCP 使用、Code Review、测试、重构、安全、性能 |
| `06-faq.md` | FAQ：常见问题速查，覆盖 Flutter / Dart / Widget / State / 架构 / AI 生成 / Prompt / Rules / Agent / MCP / Debug / UI 还原 / 性能 / 测试 / 重构 / 企业项目 |

## Flutter 在 AI FullStack Workflow 中的位置

Flutter 是 AI FullStack Workflow 的全栈实战层之一（`docs/12-flutter/`），位于基础认知与工具实操之后、企业研发之前：

```text
docs/01-ai-basic/  认知层
docs/02-models/    推理能力层
docs/03~07/        工具实操层（OpenCode / Codex / Claude Code / CodeBuddy / Pi）
docs/08-mcp/       工具能力层（MCP 协议）
docs/09-rules/     行为约束层（Rules 库）
docs/10-prompts/   任务意图层（Prompt 库）
docs/11-agent/     执行层（Agent 体系）
docs/12-flutter/   全栈实战层（本章节 Flutter 工程体系）
docs/13-android/   全栈实战层（Android）
docs/14-vue/       全栈实战层（Vue）
```

Flutter 章节把前面沉淀的 **Model + Prompt + Rules + Agent + MCP** 能力，真正落到一个可运行、可验证的移动端技术栈上。它不是孤立的技术文档，而是前面所有章节方法论在 Flutter 场景下的**工程化集成示范**。

## Flutter 与 Agent 的关系

- `agents/flutter-agent/` 沉淀了 Flutter 专属 Agent：负责需求分析、生成页面、修 Bug、Code Review、重构。
- Flutter 章节讲解 Agent 如何理解 Flutter 架构、如何在真实工程里驱动页面生成与问题定位（见 `02-architecture.md`、`03-development-workflow.md`）。
- 文档讲原理与流程，Agent 资产提供可直接加载的系统提示词与工作流定义，两者职责分离，不重复复制。

## Flutter 与 Prompt 的关系

- `prompts/flutter/` 沉淀了 Flutter 专属 Prompt 模板，如 `generate_page.md`（生成 Controller / Binding / View / Route）。
- 章节讲解如何编写高质量 Flutter 开发 Prompt、如何用 `【】` 占位符复用模板、如何把需求翻译成可执行任务（见 `04-ai-assisted-development.md`、`05-best-practice.md`）。

## Flutter 与 Rules 的关系

- `rules/flutter/` 用于沉淀 Flutter 编码规范，约束 AI 与开发者产出风格一致。
- 章节讲解 Rules 如何约束 Flutter 代码（命名、分层、网络封装、状态管理等），以及如何让 Agent 遵守规则（见 `05-best-practice.md`）。

## Flutter 学习路线

按以下顺序阅读：

1. **建立整体认知**：读 `01-overview.md`，理解 Flutter 在 AI FullStack Workflow 中的位置与价值。
2. **理解工程结构**：读 `02-architecture.md`，掌握 Flutter 各层职责与可选的架构方案。
3. **掌握开发流程**：读 `03-development-workflow.md`，跑通需求 → Prompt → Rules → Agent → 代码 → 测试 → Review。
4. **学会用 AI 提效**：读 `04-ai-assisted-development.md`，看 AI 如何在页面、状态管理、Debug 等场景辅助。
5. **沉淀团队实践**：读 `05-best-practice.md`，落地为团队规范。
6. **遇到问题查漏**：读 `06-faq.md`。

## 阅读建议

- **想快速了解 Flutter 章节**：读 `01-overview.md`。
- **想理解 Flutter 工程结构**：读 `02-architecture.md`。
- **想跑通一整套 Flutter 研发流程**：读 `03-development-workflow.md`。
- **想用 AI 提升 Flutter 开发效率**：读 `04-ai-assisted-development.md`。
- **想在团队落地最佳实践**：读 `05-best-practice.md`。
- **遇到具体问题**：查 `06-faq.md`。

> 注：本章节是 **Flutter AI 工程体系的知识与方法论**；实际可复用的 Prompt 与 Agent 配置分别见 `prompts/flutter/`、`agents/flutter-agent/`。三者职责分离——文档讲解如何组织 Flutter + AI 工作流，资产提供可直接复用的模板与系统提示词。
