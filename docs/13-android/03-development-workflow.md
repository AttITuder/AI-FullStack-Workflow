# Android 开发 Workflow

> 本文是 Android + AI 的"操作手册"：针对研发中的每一类典型任务，给出可复现的工作流。最终把所有流程汇入一条统一的 需求 → Prompt → Rules → Android Agent → 代码 → Build → Test → Review 链路。

---

## 1. 新功能开发 Workflow

新功能开发是最常见的任务，流程目标是在明确边界内产出可验证的代码。

```text
需求
  ↓
Context（阅读相关模块，理解现状）
  ↓
Prompt（本次功能目标 + 技术栈约束）
  ↓
Rules（编码规范）
  ↓
Android Agent（规划 → 拆分子任务）
  ↓
逐子任务生成代码 → Gradle 构建验证
  ↓
测试（单元 / Instrumented）
  ↓
Review
```

关键控制点：

1. **先读现状再写**：理解目标模块的架构、命名、既有实现，避免风格漂移。
2. **大功能拆子任务**：UI / ViewModel / 数据 / 接口分批执行，每批带构建验证。
3. **明确验收标准**：可编译、结构合规、行为符合需求。

这一流程是后续各类具体 Workflow 的共同骨架。

## 2. 页面开发 Workflow

页面开发覆盖 View（XML 或 Compose）、ViewModel、路由与数据绑定。

```text
页面需求
  ↓
确认 UI 体系（XML + View 还是 Compose）与技术栈
  ↓
Prompt：明确页面要素、交互、数据源
  ↓
生成 View / ViewModel / 状态绑定
  ↓
接入路由 / 跳转，构建验证
  ↓
Review 与真机验证
```

关键点：

- 确定 UI 体系后保持一致，不混用 XML 与 Compose。
- 页面交互交给 ViewModel，View 只做渲染与事件分发。
- 数据通过 ViewModel 的 `StateFlow` / `LiveData` 呈现加载 / 成功 / 失败三态。
- 生命周期相关处理正确（配置变更、资源释放）。

## 3. API 开发 Workflow

接口接入涉及网络层与 ViewModel / 数据层的联动。

```text
拿到接口文档（请求 / 响应结构）
  ↓
确认 Retrofit + OkHttp 统一封装
  ↓
生成 Model 与 API Service（Retrofit 接口）
  ↓
实现 Repository / ViewModel 调用
  ↓
处理错误与三态，构建 + 测试验证
```

关键点：

- **复用统一 Client 与拦截器**，不散落新建实例（对应 `rules` 约束）。
- Model 的字段与后端严格对应，注意可空性与类型。
- 错误处理完整：网络异常、超时、业务错误码都应有反馈。
- 异步用协程（`suspend` / `flow`），正确调度到主 / IO 线程。

## 4. 数据库开发 Workflow

数据库开发覆盖 Room 的 Entity / Dao / Database。

```text
明确数据模型与查询需求
  ↓
生成 Entity（@Entity）
  ↓
生成 Dao（@Dao：增删改查 / Flow 观察）
  ↓
配置 Database 与版本 / 迁移（@Database）
  ↓
在 Repository 中接入，构建 + 测试验证
```

关键点：

- Entity 字段与业务数据对应，注意类型与索引。
- Dao 方法考虑数据变化通知（Flow / LiveData 返回值）。
- 数据库版本升级时正确编写 Migration，避免数据丢失。
- 数据访问封装在 Data 层，不在 UI 直接操作。

## 5. 三方 SDK 接入 Workflow

三方 SDK（推送、支付、统计、地图等）接入需要处理依赖、初始化、权限与回调。

```text
确认 SDK 需求与文档
  ↓
在 build.gradle 添加依赖
  ↓
配置 Manifest（权限 / 组件声明）
  ↓
实现初始化（Application 或按需）
  ↓
接入业务调用与回调处理
  ↓
构建验证 + 真机验证
```

关键点：

- 依赖版本与项目（minSdk / compileSdk）兼容。
- 权限与组件声明要在 Manifest 正确配置。
- 初始化时机与应用生命周期匹配。
- 回调在主线程 / 正确线程处理，避免内存泄漏。

## 6. Bug 修复 Workflow

修复 Bug 的准则：**先定位根因，再修复**，避免"盲改碰运气"。

```text
复现 / 收集 Logcat 报错
  ↓
定位根因：逻辑？生命周期？线程？空指针？
  ↓
先复现，再最小化定位（必要时添加日志）
  ↓
修复 → 构建验证 → 回归相关功能
  ↓
补充说明与测试
```

常见 Bug 类型：

- 空指针 / Kotlin `null` 处理。
- 主线程执行耗时操作导致卡顿或 ANR。
- 生命周期处理不当导致的资源 / 状态异常。
- 异步回调导致的状态竞态。

## 7. Crash 问题 Workflow

Crash 处理要先还原栈、再分析、后修复。

```text
拿到崩溃堆栈（Java / Kotlin / native / ANR）
  ↓
定位崩溃点：类 / 方法 / 行号
  ↓
结合 Logcat 与代码确认根因
  ↓
修复 → 构建验证 → 监控回归
  ↓
沉淀为 Rule 或测试用例
```

关键点：

- 区分崩溃类型：空指针（NPE）、数组越界、类型转换、找不到类、ANR、native signal。
- 结合版本与设备信息，判断是否与兼容性或混淆相关。
- 修复后补充单元测试或监控，防止回归。

## 8. 性能优化 Workflow

性能优化需要先测量、再定位、后优化。

```text
明确性能目标（启动 / 滑动 / 内存）
  ↓
用工具测量（Profiler / Baseline / StrictMode）
  ↓
定位瓶颈（主线程阻塞 / 泄漏 / 布局过深）
  ↓
针对性优化，构建验证并复测
```

常见优化手段：

- 减少主线程工作，耗时任务移入协程 / 后台线程。
- 避免内存泄漏（静态引用、Listener 未解绑、Handler）。
- 布局浅化（减少嵌套层级）或使用 Compose 惰性列表。
- 图片加载走统一库（Coil / Glide / Fresco）并注意缓存。

原则：**性能优化是测量驱动的**，AI 应基于测量数据给建议，而不是空谈。

## 9. 重构 Workflow

重构的前提是**先给方案、确认后再动手**。

```text
识别重构目标（过大的 Activity / 重复代码 / 分层混乱）
  ↓
AI 给出重构方案（拆什么、怎么拆、影响范围）
  ↓
人工确认方案
  ↓
小步重构，每步构建验证
  ↓
测试回归，确认行为不变
```

安全重构要点：

- 每次只做一类重构，避免大爆炸式改动。
- 优先行为不变：重构不改功能，只改结构。
- 依托测试保证行为可验证。
- 重构后跑构建与单元测试。

## 10. Code Review Workflow

AI 辅助 Code Review 能降低人工审查成本，但与人工判断结合。

```text
提交变更
  ↓
AI 对照 rules/android 逐项审查
  ↓
输出结构化意见（命名 / 分层 / 线程 / 错误处理 / 性能 / 安全）
  ↓
人工复核，采纳合理意见
```

审查维度：

- 命名与 Kotlin / Java 规范。
- 分层是否清晰、职责是否单一。
- 线程与生命周期处理是否正确。
- 是否复用统一封装（网络、数据、依赖注入）。
- 性能隐患与安全风险（敏感信息、权限）。

`05-best-practice.md` 有更系统的审查清单。

## 11. Android Agent Workflow

`agents/android-agent` 把通用流程固化为公用的 Android 工作流（配置内容在资产中持续完善）：

```text
需求 → 分析 → 设计 → 生成 → 自检 → 交付
```

### 11.1 需求分析

明确页面 / 功能目标，确认技术栈（Kotlin / Java、XML / Compose、状态管理、网络）与架构约束，输出任务拆解清单。

### 11.2 设计

确定数据模型与接口、ViewModel / 状态设计、模块归属，输出文件结构。

### 11.3 生成

按模块生成代码（View / ViewModel / Repository / Model），复用统一封装，遵循项目技术栈。

### 11.4 自检

对照验收清单逐项检查：可编译、分层正确、命名统一、无硬编码、错误处理与线程正确。

### 11.5 交付

说明新增 / 修改的文件，说明需执行的命令（如 `./gradlew assembleDebug`、`Sync`），提示后续注意事项。

## 12. Prompt + Rules + Agent Workflow

把前面所有流程汇总为统一执行链。它回答了"**Android + AI 到底怎么跑通一次开发**"。

```text
需求
  ↓
Prompt（把需求翻译成明确、带约束的任务）
  ↓
Rules（加载编码规范，作为生成与验收依据）
  ↓
Android Agent（进入执行闭环，理解项目结构后动手）
  ↓
代码（分模块产出，遵循分层）
  ↓
Build（Gradle 构建验证）
  ↓
Test（单元 / Instrumented 测试）
  ↓
Review（对照 Rules 结构化审查）
```

各环节的职责：

- **Prompt**：负责"意图"，把模糊需求变成可执行任务，用 `【】` 占位符复用模板。
- **Rules**：负责"纪律"，约束生成与验收标准，保证风格一致。
- **Android Agent**：负责"执行"，理解项目结构、拆解任务、生成 / 自检 / 交付。
- **Model**：负责"推理与生成"，是能力底座。
- **MCP / Tools**：负责"触达外部"，连接构建、设备、设计稿等。
- **Build / Test / Review**：负责"验收"，保证质量闭环。

这一条链就是 `01-overview.md` 中 Android AI Workflow 的可执行版本。把每一步都标准化、可复用，Android 开发的效率与质量就能从"碰运气"变成"可复制"。

## 总结

- 新功能、页面、API、数据库、三方 SDK、Bug、Crash、性能、重构、Review，每类任务都有明确可复现的 Workflow。
- 统一原则：**先读后写、先方案后动手、可验证、遵循 Rules、重视线程与生命周期**。
- 所有流程最终汇入 需求 → Prompt → Rules → Android Agent → 代码 → Build → Test → Review 的统一链路。
- `agents/android-agent` 是这套工作流的资产化固化，一次配置、长期复用。

## 参考资料

- `agents/android-agent/`：Android Agent 配置与职责。
- `prompts/android/`：Android 开发 Prompt 模板规划。
- `rules/android/`：编码与审查约束。
- `docs/11-agent/04-workflow.md`：通用 Agent Workflow 方法论。
- `docs/12-flutter/03-development-workflow.md`：同层级 Flutter Workflow 章节。
