# AI 辅助 Android 开发

> 本文聚焦"AI 到底能在 Android 开发里做什么、怎么用"，从具体场景出发，说明 AI 如何与 Agent、MCP、Rules、Prompt 配合，提升 Android 开发效率。

---

## 1. AI 辅助 Android 场景

### 1.1 项目分析

AI 先读懂整个 Android 项目（模块结构、`build.gradle`、技术栈、分层）再辅助开发，降低"盲写"风险。

### 1.2 页面开发

AI 把需求描述生成页面代码（Activity / Fragment / ViewModel，或 Compose 组件），并接入路由、状态与数据。

### 1.3 Kotlin 代码生成

现代 Android 主力语言。AI 能生成 `data class`、`ViewModel`、ViewModel 等结构清晰、协程安全的 Kotlin 代码。

### 1.4 Java 代码维护

存量项目常含 Java 代码。AI 需能维护、重构、迁移 Java 代码，注意与 Kotlin 混编的项目规范。

### 1.5 Compose 开发

AI 能根据描述生成 `@Composable` 组件、状态收集与导航。Compose 声明式风格对 AI 友好，还原度高。

### 1.6 XML 布局

AI 能生成或修改 XML 布局（`ConstraintLayout`、`RecyclerView` 等），处理布局层级与适配。

### 1.7 Debug

AI 结合 Logcat、堆栈与代码定位 Bug，给出根因假设与修复建议。

### 1.8 Crash 分析

AI 解析崩溃堆栈，识别崩溃类型与源头代码，给出修复方向。

### 1.9 性能优化

AI 基于测量数据给性能方案：主线程、内存泄漏、布局、图片、启动等。

### 1.10 重构

AI 拆解过大的 Activity / Fragment、提取公共组件、规范化分层。遵循"先方案、后动手"。

## 2. AI 理解 Android 代码

AI 生成 / 修改代码前，需要先"理解"当前代码。

### 2.1 理解维度

- **组件与生命周期**：Activity / Fragment / Service 的状态与生命周期。
- **UI 体系**：是 XML + View 还是 Compose。
- **状态与数据流**：`StateFlow` / `LiveData`，数据从哪来、到哪去。
- **依赖与模块**：模块边界、`build.gradle` 依赖。
- **线程模型**：主 / IO / 后台线程的调度。

### 2.2 让 AI 更好理解

- 提供模块结构、`build.gradle`、Manifest 作为 Context。
- 保持命名与分层一致，降低理解成本。
- 用 `/` 注释说明关键职责。
- 让 AI"复述"理解后再动手，确认理解正确。

## 3. AI 辅助架构设计

AI 可以作为架构方案的"参谋"。

### 3.1 给出可选方案

针对项目规模，AI 给出分层与状态管理选型的利弊对比，而不是武断选一个框架。

### 3.2 分析现状与缺口

AI 分析当前结构的职责是否清晰、依赖是否合理、模块划分是否妥当，指出重构点。

### 3.3 重要原则

- **不强制单一方**：MVVM、Clean Architecture、Compose 都只是选项（见 `02-architecture.md`）。
- **人拍板**：架构是团队决策，AI 只提供分析建议。
- **先方案后动手**：重大变更须人工确认。

## 4. AI 辅助问题定位

AI 在 Debug 与 Crash 场景的价值在于"快速缩小范围"。

### 4.1 输入组织

提供完整信息：崩溃堆栈、Logcat 关键日志、相关代码、复现步骤、设备与版本。

### 4.2 输出形式

- 崩溃 / 异常类型判断（NPE、ANR、native 等）。
- 根因候选与依据。
- 修复方案与验证方式。
- 需要补充的信息（如果信息不足，主动询问）。

### 4.3 注意点

- 让 AI"先定位根因，再修复"，避免盲改。
- 复杂问题结合人工经验（尤其厂商适配类）。
- 修复后通过构建与测试验证，必要时补充用例防回归。

## 5. AI + Android Agent

Agent 是把 AI 与 Android 工作流"资产化"的关键。

### 5.1 Agent 带来的价值

- 一次配置、长期复用一致的 Android 协作体验。
- 固化了"先理解项目 → 生成 → 自检 → 交付"的纪律（`需求 → 分析 → 设计 → 生成 → 自检 → 交付`）。
- 约束了场景动作：生成页面、修 Bug、Review、重构各有约定。

### 5.2 与纯 Chat 的区别

- 纯 Chat：每次都要重新描述角色、技术栈、目标。
- 用 Agent：加载 `agents/android-agent`，命令式地委派任务，AI 自主按流程执行。

### 5.3 使用方式

将 `agents/android-agent` 的配置作为系统提示词加载到 AI 编码工具（OpenCode / Claude Code 等），在项目中开启使用。

## 6. AI + MCP

MCP 让 AI 触达外部工具与数据，增强 Android 开发流程（见 `docs/08-mcp/` 与 `mcp/`）。

### 6.1 MCP 在 Android 开发中的价值

- **ADB**：连接设备 / 模拟器，运行、查看 Logcat、调试。
- **Gradle / 构建**：获得构建与打包反馈。
- **GitHub / Git**：管理分支与变更、辅助 Review。
- **Figma**：读取设计稿，辅助 UI / Compose 还原。
- **文档 / 接口**：作为生成 Context。

### 6.2 MCP 与 Android 的结合点

- 持续验证：构建 / ADB MCP 提供反馈 → 迭代代码。
- UI 还原：Figma MCP 提供设计数据 → 生成对应 UI。
- 信息获取：接口 / 规范文档 MCP → 作为 Prompt Context。

## 7. AI + Rules

Rules 让 AI 产出"符合团队预期"的代码（见 `docs/09-rules/` 与 `rules/android/`）。

### 7.1 Rules 的作用

- 统一 Kotlin / Java 命名与风格。
- 约束分层、网络层、数据层、依赖注入方式。
- 强调线程与生命周期约定。
- 作为生成与 Review 的验收标准。

### 7.2 让 AI 遵守 Rules 的方法

- 在 Agent 系统提示词中引用 Rules。
- 在 Prompt 中要求对照 Rules 生成。
- 在 Review 阶段让 AI 按 Rules 逐项核对。

### 7.3 高质量 Rules 的特点

可执行、可检查、不模糊。没有 Rules，AI 输出风格千差万别。

## 8. AI + Prompt

Prompt 是把需求翻译成任务的关键（见 `docs/10-prompts/` 与 `prompts/android/`）。

### 8.1 好 Prompt 的要素

- 角色设定（资深 Android 工程师）。
- 技术栈约束（Kotlin / Java、XML / Compose、MVVM / 其他、网络方案）。
- 明确任务与范围。
- 输出要求与验收清单。
- 用 `【】` 占位符实现可复用。

### 8.2 Prompt 与 Agent 的关系

Agent 提供"身份与纪律"，Prompt 提供"本次任务"。两者配合：Agent 决定"你是谁、怎么工作"，Prompt 决定"这次干什么"。

### 8.3 Context 的重要性

好的生成依赖充分的 Context：相关代码、接口文档、架构说明、`build.gradle`。Context 不足会导致 AI 产出偏离现状。

## 9. 企业 Android 项目 AI Workflow

在企业项目中，AI 的价值不止于"单点提效"，而是整条研发流水线。

```text
需求管理 → 任务拆解 → 代码生成 → Build → Test → Review → 发布（Release）
```

### 9.1 统一资产

- 团队统一的 Prompt 库（`prompts/android/`）。
- 统一的 Rules 库（`rules/android/`）。
- 统一的 Agent（`agents/android-agent/`）。
- 统一的 MCP 配置（`mcp/`）。

### 9.2 团队协作要点

- 产出风格一致、质量可预期。
- 交接成本降低：新人加载同一套资产即可上手。
- 规范可传承：最佳实践沉淀为资产，而非个人经验。

### 9.3 质量闭环

- Code Review 由 AI 辅助 + 人工拍板。
- 构建链路（Gradle）与测试体系（单元 / Instrumented）建立。
- 安全与合规红线约束（见 `05-best-practice.md`）。

### 9.4 发布链路

- 版本管理、签名、多渠道、混淆规则在 Rules 与流程中沉淀。
- AI 辅助生成构建配置与发布检查项，但关键发布决定由人掌控。

### 9.5 落地路径

从"个人用 AI 提效"起步，逐步沉淀团队资产，最终形成稳定、可复现的企业级 Android AI 研发流水线。

## 总结

- AI 在 Android 开发中的价值覆盖项目分析、页面开发、Kotlin / Java / Compose / XML、Debug、Crash 分析、性能优化、重构。
- Agent、MCP、Rules、Prompt 各司其职：Agent 负责执行纪律，MCP 连接外部能力，Rules 约束产出，Prompt 传递任务。
- 企业落地靠统一资产沉淀与构建 / 测试 / Review / 发布的质量闭环。

## 参考资料

- `prompts/android/`：Android 开发 Prompt 模板规划。
- `rules/android/`：Android 编码规范。
- `agents/android-agent/`：Android Agent 资产。
- `mcp/`、`docs/08-mcp/`：MCP 配置与方法论。
- `docs/09-rules/`、`docs/10-prompts/`、`docs/11-agent/`：前置方法论。
