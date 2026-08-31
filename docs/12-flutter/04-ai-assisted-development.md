# AI 辅助 Flutter 开发

> 本文聚焦"AI 到底能在 Flutter 开发里做什么、怎么用"，从具体场景出发，说明 AI 如何与 Agent、MCP、Rules、Prompt 配合，提升 Flutter 开发效率。

---

## 1. AI 辅助 Flutter 的场景

### 1.1 页面开发

AI 把需求描述翻译成完整的页面代码（View / Controller / Binding / Route）。这是最直接、价值最高的场景，复用 `prompts/flutter/generate_page.md`。

### 1.2 UI 还原

把设计稿或视觉描述还原成 Widget 树。AI 能根据描述生成间距、颜色、布局等，结合 MCP 的 Figma 能力可读取设计稿数据（见 `mcp/figma/`）。

### 1.3 状态管理

AI 辅助设计状态流转：三态模型、跨页面共享、异步更新时机。它让人从"想清楚流程"中解放，专注于校验正确性。

### 1.4 Debug

AI 定位报错根因、分析异常行为，提供修复建议与最小复现思路。`flutter-agent` 强制"先定位根因，再修复"。

### 1.5 重构

AI 拆解巨型 Widget、提取公共组件、规范命名、理清分层。重构遵循"先方案、后动手"。

### 1.6 测试

AI 生成单元测试与 Widget 测试，覆盖状态与交互逻辑，辅助建立测试体系。

## 2. AI 分析 Flutter 项目

AI 可以作为"项目顾问"，先读懂整个项目再辅助开发。

### 2.1 分析输入

提供：目录结构、`pubspec.yaml`、路由配置、关键模块代码。让 AI 输出项目概览与架构理解。

### 2.2 分析输出

- **技术栈识别**：状态管理、网络、存储用了什么方案。
- **模块划分**：业务边界的组织方式。
- **潜在问题**：职责不清、重复代码、耦合过高等。
- **开发建议**：新功能的最佳落点。

### 2.3 分析的价值

分析让 AI "先有全局、再写局部"，降低生成代码与现状不符的风险。分析结果用于后续页面生成、Bug 定位、重构的 Context 基础。

## 3. AI 生成 Flutter 页面

典型生成流程（对应 `03-development-workflow.md` 第 2 节）：

```text
页面需求
  ↓
组织 Context（相关模块 / 架构 / 技术栈）
  ↓
使用 generate_page Prompt 模板
  ↓
AI 生成 View / Controller / Binding / Route
  ↓
注册路由，构建验证，Review
```

### 3.1 高质量页面生成的条件

- **明确需求**：页面要素、交互、接口、跳转目标。
- **技术栈一致**：状态管理 / 网络 / 架构与项目对齐。
- **可验证**：`flutter analyze` + 构建通过。

### 3.2 常见问题与规避

- 结构不符项目分层 → 用 Rules + sample 约束。
- 路由未注册 → 验收清单覆盖。
- 过度生成 / 偏离需求 → 明确范围约束。

## 4. AI 优化 Flutter 代码

AI 可以在不改变行为的前提下优化代码质量。

### 4.1 代码质量优化

- 拆分巨型 Widget，提升可读性。
- 提取公共组件与工具，减少重复。
- 用 `const` 提升构建性能。
- 统一命名与格式（`dart format`）。

### 4.2 逻辑优化

- 简化复杂条件与重复逻辑。
- 完善错误处理与空安全。
- 规范异步代码（`async` / `await` / 错误兜底）。

优化后的代码必须经过 `flutter analyze` 与构建 / 测试验证，确保行为不变。

## 5. AI 辅助 Flutter 架构设计

AI 可以作为架构方案的"参谋"：

### 5.1 给出可选方案

针对项目规模，AI 给出分层与状态管理选型的利弊对比，而不是武断地选一个框架。

### 5.2 分析现状与缺口

AI 分析当前结构的职责是否清晰、依赖是否合理，指出重构点。

### 5.3 重要原则

- **不强制单一方**：GetX / Bloc / Provider 都只是选项（见 `02-architecture.md`）。
- **人拍板**：架构是团队决策，AI 只提供分析与建议。
- **先方案后动手**：重大变更须人工确认。

## 6. AI + Flutter Agent

Agent 是把 AI 与 Flutter 工作流"资产化"的关键。

### 6.1 Agent 带来的价值

- 一次配置、长期复用一致的 Flutter 协作体验。
- 固化了"先理解架构 → 生成 → 自检 → 交付"的纪律。
- 约束了场景动作：生成页面、修 Bug、Review、重构各有约定。

### 6.2 与纯 Chat 的区别

- 纯 Chat：每次都要人重新描述角色、技术栈、目标。
- 用 Agent：加载 `flutter-agent`，命令式地委派任务，AI 自主按流程执行。

### 6.3 使用方式

将 `agents/flutter-agent/prompt.md` 作为系统提示词加载到 AI 编码工具（OpenCode / Claude Code 等），在项目中开启使用。

## 7. AI + MCP

MCP（Model Context Protocol）让 AI 触达外部工具与数据，增强 Flutter 开发流程（见 `docs/08-mcp/` 与 `mcp/`）。

### 7.1 MCP 在 Flutter 开发中的价值

- **Figma**：读取设计稿，辅助 UI 还原。
- **GitHub / Git**：管理分支、查看变更、辅助 Review。
- **ADB / 构建**：连接构建与调试工具。
- **文档 / 项目**：读取规范与设计文档，作为生成 Context。

### 7.2 MCP 与 Flutter 的结合点

- UI 还原：Figma MCP 提供设计数据 → 生成对应 Widget。
- 持续验证：构建 / 测试 MCP 提供反馈 → 迭代代码。
- 信息获取：接口文档 / 设计文档 MCP → 作为 Prompt Context。

## 8. AI + Rules

Rules 让 AI 产出"符合团队预期"的代码（见 `docs/09-rules/` 与 `rules/flutter/`）。

### 8.1 Rules 的作用

- 统一命名、分层、封装方式。
- 约束网络层、状态管理、路由的使用。
- 作为生成与 Review 的验收标准。

### 8.2 让 AI 遵守 Rules 的方法

- 在 Agent 系统提示词中引用 Rules（如 `flutter-agent/prompt.md` 声明"遵循 projects 下 rules 规范"）。
- 在 Prompt 中要求对照 Rules 生成。
- 在 Review 阶段让 AI 按 Rules 逐项核对。

### 8.3 高质量 Rules 的特点

可执行、可检查、不模糊（命名规范、分层要求、封装约定等）。没有 Rules，AI 输出风格千差万别。

## 9. AI + Prompt

Prompt 是把需求翻译成任务的关键（见 `docs/10-prompts/` 与 `prompts/flutter/`）。

### 9.1 好 Prompt 的要素

- 角色设定（资深 Flutter 工程师）。
- 技术栈约束（Flutter / GetX / Dio / 架构）。
- 明确任务与范围。
- 输出要求与验收清单。
- 用 `【】` 占位符实现可复用。

### 9.2 Prompt 与 Agent 的关系

Agent 提供"身份与纪律"，Prompt 提供"本次任务"。两者配合：Agent 决定"你是谁、怎么工作"，Prompt 决定"这次干什么"。

### 9.3 Context 的重要性

好的生成依赖充分的 Context：相关代码、接口文档、架构说明。Context 不足会导致 AI 产出偏离现状。

## 10. 企业 Flutter 项目 AI Workflow

在企业项目中，AI 的价值不止于"单页生成"，而是整条研发流水线。

```text
需求管理 → 任务拆解 → 代码生成 → 测试 → Review → 发布
```

### 10.1 统一资产

- 团队统一的 Prompt 库（`prompts/flutter/`）。
- 统一的 Rules 库（`rules/flutter/`）。
- 统一的 Agent（`agents/flutter-agent/`）。
- 统一的 MCP 配置（`mcp/`）。

### 10.2 团队协作要点

- 产出风格一致、质量可预期。
- 交接成本降低：新人加载同一套资产即可上手。
- 规范可传承：最佳实践沉淀为资产，而非个人经验。

### 10.3 质量闭环

- Code Review 由 AI 辅助 + 人工拍板。
- 测试体系（单元 / Widget / 集成）逐步建立。
- 安全与合规红线上限约束（见 `05-best-practice.md`）。

### 10.4 落地路径

从"个人用 AI 提效"起步，逐步沉淀团队资产，最终形成稳定、可复现的企业级 Flutter AI 研发流水线。

## 总结

- AI 在 Flutter 开发中的价值覆盖页面开发、UI 还原、状态管理、Debug、重构、测试。
- Agent、MCP、Rules、Prompt 各司其职：Agent 负责执行纪律，MCP 连接外部能力，Rules 约束产出，Prompt 传递任务。
- 企业落地靠统一资产的沉淀与团队协作的质量闭环。

## 参考资料

- `prompts/flutter/`：可复用的 Flutter Prompt 模板。
- `rules/flutter/`：Flutter 编码规范。
- `agents/flutter-agent/`：Flutter Agent 资产。
- `mcp/`、`docs/08-mcp/`：MCP 配置与方法论。
- `docs/09-rules/`、`docs/10-prompts/`、`docs/11-agent/`：前置方法论。
