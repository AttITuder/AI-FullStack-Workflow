# Rules

Rules 是 AI FullStack Workflow 中的**约束层**：通过结构化规则约束 AI Coding Agent 的长期行为，让产出稳定、一致、可预期。

本章节介绍：

- Rules 定位与核心概念
- 使用与接入方式
- Rule 结构设计方法
- 规则驱动的开发 Workflow
- 团队最佳实践
- 常见问题

## 目录

- `01-overview.md` — Rules 概述
- `02-install.md` — Rules 使用与接入
- `03-rule-structure.md` — Rules 结构设计
- `04-workflow.md` — Rules Workflow
- `05-best-practice.md` — 最佳实践
- `06-faq.md` — FAQ

## Rules 是什么

Prompt 告诉 AI"这一次做什么"，Rules 告诉 AI"长期应该怎么做"。Rules 以文件形式沉淀团队约定，Agent 加载后自动遵循——同样的任务无论谁发起、何时发起，AI 行为方式一致。

在完整工作流中：

```text
Model（能力来源）
   ↓
Agent（规划与执行）
   ↓
MCP（工具连接）
   ↓
Rules（行为约束）
   ↓
Workflow（稳定流程）
```

MCP 给 Agent 能力，Rules 给能力划边界；Agent 定义角色，Rules 定义纪律。

## 当前项目 Rules 资产

规则资产存放在项目根目录 `rules/`，按技术栈与场景组织为八个分类：

| 分类 | 职责 |
| --- | --- |
| `android/` | Android 平台编码规范 |
| `kotlin/` | Kotlin 语言规范 |
| `java/` | Java 语言规范 |
| `dart/` | Dart 语言规范 |
| `flutter/` | Flutter 编码规范 |
| `vue/` | Vue 编码规范 |
| `git/` | Git 提交与分支规范 |
| `review/` | 代码审查规范 |

注意区分两个层面：`docs/09-rules/`（本章节）是知识体系与方法论，回答"怎么设计、怎么管理"；`rules/` 是实际可复用的规则资产，存放规则条文本身。新增或修改规则资产时以本章节方法论为准绳。

## 阅读建议

1. 先读 `01-overview.md` 建立整体认知。
2. 按 `02-install.md` 了解 Rules 如何接入 Agent 环境。
3. 参考 `03-rule-structure.md` 掌握 Rule 设计方法。
4. 按 `04-workflow.md` 建立规则驱动的开发流程。
5. 沉淀团队实践参照 `05-best-practice.md`。
6. 遇到问题查 `06-faq.md`。
