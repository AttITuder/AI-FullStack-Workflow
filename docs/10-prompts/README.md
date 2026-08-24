# 10-prompts

> Prompt（提示词）知识体系。覆盖 Prompt 的定位与边界、结构设计方法、场景模式、工作流编排、团队资产管理实践与常见问题。

---

## 本章节内容

| 文档 | 内容 |
| --- | --- |
| `01-overview.md` | 概述：Prompt 是什么、为什么 AI Coding 需要它、在六层架构中的位置、与 Rules / MCP / Agent / Context 的边界 |
| `02-prompt-structure.md` | 结构设计：好 Prompt 的八要素（Role / Context / Goal / Task / Constraints / Input / Output / Validation）、长度取舍、信息优先级、结构模板 |
| `03-prompt-patterns.md` | Patterns：16 种 Prompt 模式，每种按"适用场景 → 提示策略 → 预期效果"展开 |
| `04-workflow.md` | Workflow：Prompt 驱动研发全流程的六个环节、完整功能开发示例、会话管理策略 |
| `05-best-practice.md` | 最佳实践：Prompt 资产化管理的二十条原则——编写、验证、迭代、演进、安全 |
| `06-faq.md` | FAQ：34 个常见问题速查 |

## 核心观点

- **Prompt 是任务意图层**：描述这次做什么、基于什么现状、遵守什么限制、交付什么结果。它的质量是 AI Coding 产出质量的第一道闸门。
- **八要素是清单不是表单**：按任务复杂度裁剪，缺失项必须是有意识省略。
- **模式应对失败**：风格漂移用 Context、盲改用根因先行、范围蔓延用单轮聚焦——每个 Pattern 背后是对一类失败模式的针对性设计。
- **流程化让 Prompt 变短**：前一环节的产出固化为下一环节的输入，单个 Prompt 无需背负全部上下文。
- **资产化沉淀能力**：`prompts/` 目录存放实测通过的团队资产；反复手写的 Prompt 应升级为 Agent 配置或下沉为 Rules。

## 与其他章节的关系

```text
docs/08-mcp/      工具能力层 —— MCP 提供 Agent 触达外部工具的通道
docs/09-rules/    行为约束层 —— Rules 管长期行为，Prompt 管单次任务
docs/10-prompts/  任务意图层 —— 本章节
docs/11-agent/    执行层     —— Agent 是 Prompt 的宿主与放大器
```

阅读顺序建议：先读本章节理解意图层，再读 Rules 章节理解约束层，两者对照最能体会分工。

## 阅读建议

- **想快速上手**：读 `02-prompt-structure.md` 掌握八要素，配合标准范例 `prompts/flutter/generate_page.md` 对照学习。
- **想系统掌握**：按编号顺序通读，`03-prompt-patterns.md` 和 `04-workflow.md` 是核心方法文章。
- **团队管理者**：直接看 `05-best-practice.md`，重点是资产化原则与问题上升机制。
- **遇到具体问题**：查 `06-faq.md`。

> 注：本章节讲解方法论，不存放可复制的 Prompt 正文。实际可用的 Prompt 资产见 `prompts/` 目录。
