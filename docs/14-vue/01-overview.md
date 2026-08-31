# Vue AI 开发概述

> 本章节是 Vue 工程体系的入口。它回答三个根本问题：Vue 在 AI FullStack Workflow 中处于什么位置、Vue 技术生态与前端工程特点，以及 Vue + AI 的整体工作流是什么样。

---

## 1. Vue 在 AI FullStack Workflow 中的位置

AI FullStack Workflow 的内容分为几条主线：基础认知（`docs/01` 到 `docs/02`）、工具实操（`docs/03` 到 `docs/07`）、全栈实战（`docs/12` 到 `docs/14`）、企业研发（`docs/15` 与 `docs/16`）。Vue 章节属于**全栈实战层**，与 Flutter、Android 并列，是前面所有方法论在前端技术栈上的落地示范。

从能力层级看，Vue 章节把 AI FullStack Workflow 的核心资产串了起来：

```text
Model      ——  提供推理与代码生成能力
Prompt     ——  prompts/vue/ 提供页面 / 组件 / UI 还原等任务模板
Rules      ——  rules/vue/ 提供编码与审查约束
Agent      ——  agents/ 提供架构 / Review / Test 等可复用 Agent
MCP        ——  mcp/ 提供连接接口文档、设计稿、数据等能力
      ↓
Vue 工程（本章节覆盖的架构、Workflow、最佳实践）
```

关键认知：**Vue 章节不是再教一遍 Vue 语法，而是把"如何用 AI 高效构建 Vue 前端工程"沉淀为可复用、可团队复现的工作流**。这与项目定位（`docs/00-roadmap/project-positioning.md`）一致——沉淀工作流，而非追逐工具版本。

## 2. Vue 技术生态

要组织 Vue + AI 工作流，需要先理解 Vue 生态的核心组成。

### 2.1 Vue3

Vue 3 是当前的现代版本，采用 Composition API 与响应式系统，组件灵活、生态成熟。AI 生成代码时默认面向 Vue 3 组合式写法。

### 2.2 Composition API

组合式 API 通过 `setup`、`ref`、`reactive`、`computed`、`watch` 组织逻辑，把相关状态与逻辑聚合，可复用性、可读性、可测试性更强：

```ts
<script setup lang="ts">
import { ref, computed } from 'vue'
const count = ref(0)
const double = computed(() => count.value * 2)
</script>
```

### 2.3 TypeScript

TypeScript 是 Vue 项目的类型保障。AI 生成代码时若项目启用了 TS，应产出类型安全的组件与接口定义，避免 `any` 泛滥。

### 2.4 Vite

Vite 是现代 Vue 的推荐构建工具，开发启动快、热更新、构建链简单。`vite.config.ts` 管理构建、代理、插件等。

### 2.5 Vue Router

Vue Router 负责页面导航与路由管理。AI 生成页面时要同步产出路由配置，否则页面无法被访问。

### 2.6 Pinia

Pinia 是 Vue 3 推荐的**状态管理**方案，基于 Composition API，简约、类型安全。取代旧的 Vuex。

### 2.7 UI 组件库

Element Plus、Ant Design Vue、Naive UI、Vant 等组件库覆盖常用 UI。AI 生成时需按项目组件库命名与使用方式产出，保持一致。

### 2.8 CSS 方案

Tailwind CSS、SCSS、CSS Modules、CSS-in-JS 等方案并存。AI 生成的样式需符合项目选定的 CSS 方案。

## 3. Vue 前端工程特点

### 3.1 组件化

Vue 的核心是组件。页面由基础组件、业务组件、页面组件组合而成。AI 生成时要保持组件边界清晰、可复用、职责单一。

### 3.2 响应式

Vue 采用响应式数据驱动视图。AI 需要正确理解 `ref` / `reactive`、`computed` 派生数据、`watch` 副作用，避免用错导致界面不更新或重复执行。

### 3.3 状态管理

跨页面共享状态用 Pinia。AI 生成时要区分"组件局部状态"与"全局状态"，避免滥用全局 store。

### 3.4 工程化

Vue 前端工程涉及构建（Vite）、类型（TS）、规范（ESLint / Prettier）、测试、CI。AI 生成的代码需要纳入这套工程体系。

### 3.5 构建体系

构建链（依赖、构建、产物、发布）是 Vue 工程的基石。`npm install` / `npm run build` / lint 是 AI 生成后最先获得的验证反馈。

## 4. AI 如何改变 Vue 开发

### 4.1 页面生成

AI 把需求描述生成完整的 Vue 页面（模板 + 逻辑 + 样式 + 路由）。这是价值最高、最成熟的场景。

### 4.2 组件生成

AI 能生成可复用的基础组件与业务组件，保证接口（props / emits）清晰、风格一致。

### 4.3 UI 还原

AI 能把设计稿或视觉描述还原成 Vue 模板与样式，结合 MCP 的 Figma 能力读取设计稿数据。

### 4.4 Bug 分析

AI 能结合报错信息（控制台 / 网络 / 组件警告）快速定位问题，给出根因假设与修复建议。

### 4.5 重构

AI 能帮助拆分过大组件、提取公共逻辑（`composables`）、统一命名、规范化状态管理。遵循"先方案、后动手"。

### 4.6 测试

AI 能生成单元测试（Vitest / Jest）与组件测试，覆盖组件渲染、事件与状态流转。

## 5. Vue 与 Prompt 的关系

`prompts/vue/` 用于沉淀可直接复用的 Vue 开发 Prompt（当前规划中）。应覆盖页面生成、组件生成、UI 还原、接口联调、状态管理、Bug 修复等模板，用 `【】` 占位符实现复用。

高质量 Vue Prompt 应包含：**角色设定**（资深 Vue 前端工程师）、**技术栈约束**（Vue3 / Composition API / TS / Vite / Pinia / UI 库 / CSS 方案）、**明确任务**（生成什么页面 / 组件）、**输出要求**（文件、结构、验收清单）。Prompt 是把"需求翻译成任务"的载体。

## 6. Vue 与 Rules 的关系

`rules/vue/` 用于沉淀 Vue 编码规范，是让 AI 与团队产出风格一致的"行为准则"。覆盖范围示例：

- Vue3 / Composition API 写法规范。
- TypeScript 类型约束。
- 组件命名、文件结构、props / emits 约定。
- 状态管理使用方式（Pinia，何时用全局 / 局部状态）。
- CSS 方案与样式命名。
- 是否存在硬编码、重复代码、未处理的错误。

Rules 的关键是**可执行、可检查**。有了 Rules，AI 才能稳定地产出符合团队预期的 Vue 代码，人工 Review 也更有依据。

## 7. Vue 与 Agent 的关系

Vue 前端开发由可复用的 Agent 辅助。项目 `agents/` 已沉淀 `architect-agent`（架构）、`review-agent`（Review）、`test-agent`（测试）、`product-agent`（需求）等通用与流程 Agent，可在 Vue 工程中组合使用：

- **架构 / 页面生成**：理解 Vue 项目结构后生成组件与页面。
- **Review**：对照 Rules 审查代码质量。
- **测试**：辅助生成单元与组件测试。
- **产品**：帮助把需求澄清为目标。

理解：**Agent 是前面的 Model / Prompt / Rules / MCP 的编排者**。开发者不必每次重新描述"你要当资深 Vue 工程师"，加载 Agent 即可获得一致的协作体验。本章节更多内容见 `02-architecture.md`（Agent 如何理解 Vue 项目）与 `03-development-workflow.md`（Prompt + Rules + Agent Workflow）。

## 8. Vue 与 MCP 的关系

MCP 让 AI 连接外部工具与数据（见 `docs/08-mcp/` 与 `mcp/`）。在 Vue 开发中的价值：

```text
Figma MCP           ——  读取设计稿，辅助 UI / 组件还原
文档 / 接口 MCP     ——  规范与接口作为生成 Context
GitHub / Git MCP    ——  版本管理与变更查看
数据库 / 数据 MCP   ——  联调数据作为参考
```

MCP 让 AI 获取"真实世界"的反馈与数据，是打通"AI 与 Vue 前端开发 / 联调闭环"的关键。

## 9. Vue AI Workflow

把模型、Prompt、Rules、Agent、MCP 整合成 Vue 开发的可复现流程：

```text
需求
  ↓
Prompt（把需求翻译成明确任务）
  ↓
Rules（加载编码与审查约束）
  ↓
Frontend Agent（进入执行闭环）
  ↓
Model（推理与生成代码）
  ↓
MCP / Tools（连接接口、设计稿等能力）
  ↓
Vue 代码
  ↓
测试（单元 / 组件 / lint / build）
  ↓
Review（对照 Rules 验收）
  ↓
Release（发布）
```

这条流程是 `03-development-workflow.md` 的骨架。每个环节都可独立优化，整条链路追求"**稳定、可复现、高质量**"。

## 10. 本章节学习路线

| 章节 | 主题 | 解决什么问题 |
| --- | --- | --- |
| `01-overview.md` | Vue AI 开发概述 | 建立整体认知与位置感（本章） |
| `02-architecture.md` | Vue 工程架构 | 理解各层职责，掌握可选的架构方案 |
| `03-development-workflow.md` | Vue 开发 Workflow | 跑通从需求到交付的完整流程 |
| `04-ai-assisted-development.md` | AI 辅助 Vue 开发 | 学习 AI 在各场景的具体用法 |
| `05-best-practice.md` | Vue 最佳实践 | 落地为团队可执行的规范 |
| `06-faq.md` | FAQ | 遇到问题时快速查漏 |

推荐阅读顺序：

1. 先读本章，理解 Vue 在 AI FullStack Workflow 中的位置。
2. 接着读 `02-architecture.md`，建立工程结构认知。
3. 然后按 `03-development-workflow.md` 走通一整套流程。
4. 再深入 `04-ai-assisted-development.md` 掌握 AI 具体用法。
5. 在团队落地时参照 `05-best-practice.md`。
6. 遇到问题查询 `06-faq.md`。

## 总结

- Vue 位于 AI FullStack Workflow 的**全栈实战层**，是 Model / Prompt / Rules / Agent / MCP 能力的工程化集成示范。
- Vue 技术生态包括 Vue3 / Composition API / TypeScript / Vite / Vue Router / Pinia / UI 组件库 / CSS 方案；工程特点是组件化、响应式、状态管理、工程化、构建体系。
- Vue + AI 的价值点是页面生成、组件生成、UI 还原、Bug 分析、重构、测试。
- 核心是做出一套 **需求 → Prompt → Rules → Agent → Vue 代码 → 测试 → Review → Release** 的可复用工作流。

## 参考资料

- `docs/00-roadmap/project-positioning.md`、`project-architecture.md`：项目定位与目录职责。
- `docs/08-mcp/` 到 `docs/11-agent/`：MCP、Rules、Prompt、Agent 的前置方法论。
- `docs/12-flutter/`、`docs/13-android/`：同层级的全栈实战章节。
- `prompts/vue/`、`rules/vue/`、`agents/`：Vue 相关可复用资产。
