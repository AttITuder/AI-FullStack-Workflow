# Flutter 开发 Workflow

> 本文是 Flutter + AI 的"操作手册"：针对研发中的每一类典型任务，给出可复现的工作流。最终把所有流程汇入一条统一的 需求 → Prompt → Rules → Flutter Agent → 代码 → 测试 → Review 链路。

---

## 1. Flutter 新功能开发流程

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
Flutter Agent（规划 → 拆分子任务）
  ↓
逐子任务生成代码 → 构建验证
  ↓
测试与 Review
```

关键控制点：

1. **先读现状再写**：理解目标模块的架构、命名、既有实现，避免风格漂移。
2. **大功能拆子任务**：页面 / 逻辑 / 接口分批执行，每批带验证。
3. **明确验收标准**：可编译、结构合规、行为符合需求。

这一流程是后续各类具体 Workflow 的共同骨架。

## 2. 页面开发 Workflow

页面开发是 Flutter + AI 最成熟的场景，直接复用 `prompts/flutter/generate_page.md` 模板。

```text
页面需求
  ↓
确认技术栈（GetX / Bloc / Provider 等）与架构
  ↓
Prompt：按 generate_page 模板填充【页面名称】与需求
  ↓
生成 View / Controller / Binding / Route
  ↓
注册路由，构建验证
  ↓
对照验收清单 Review
```

`generate_page.md` 的期望输出结构：

```text
lib/
├── app/routes/
├── modules/<页面>/
│   ├── controller/
│   ├── binding/
│   ├── view/
│   └── model/
└── services/api/
```

注意点：

- 页面交互事件交由 Controller 处理，View 只做渲染。
- 页面所需接口在网络层统一封装中新增。
- 路由必须正确注册，否则页面无法跳转。

## 3. API 接入 Workflow

接口接入涉及网络层与页面/数据的联动。

```text
拿到接口文档（请求 / 响应结构）
  ↓
确认是否已有统一网络封装
  ↓
生成 Model（fromJson / toJson）
  ↓
在统一 API 服务中新增方法（复用 Dio 客户端）
  ↓
在状态管理层调用，处理加载 / 成功 / 失败三态
  ↓
页面呈现数据，构建验证
```

关键点：

- **复用统一封装**，不散落新建 `Dio` 实例（对应 `rules` 约束）。
- Model 的 JSON 字段与后端严格对应，注意类型与可空性。
- 错误处理完整：网络异常、超时、业务错误码都应有反馈。
- 涉及环境差异时，BaseUrl 由配置控制。

## 4. 状态管理 Workflow

状态管理是复杂度的核心，需要想清楚"数据如何流转"再写。

```text
明确该状态的粒度（局部 / 共享）
  ↓
选择合适的方案（setState / GetX / Bloc 等）
  ↓
设计状态对象（loading / data / error 三态）
  ↓
设计更新时机与跨页面共享方式
  ↓
让页面读取状态、触发更新，构建验证
```

通用三态设计：

```dart
enum ViewStatus { loading, success, error }

class LoginState {
  final ViewStatus status;
  final User? user;
  final String? error;
}
```

建议：状态流转先"画清楚"让 AI 生成，再由人校验正确性。复杂状态用单向数据流与不可变状态更易测试。

## 5. Bug 修复 Workflow

修复 Bug 的准则：**先定位根因，再修复**，避免"盲改碰运气"。

```text
复现 / 收集报错信息
  ↓
定位根因：Widget 约束？异步竞态？类型？生命周期？
  ↓
先复现，再最小化定位（必要时添加日志）
  ↓
修复 → 构建验证 → 回归相关功能
  ↓
补充说明与测试
```

常见 Bug 类型与关注点：

- **布局 / 约束冲突**：`Unbounded constraints` 等，检查父子约束。
- **异步竞态**：未处理的 `await`、状态在 dispose 后更新。
- **类型 / 序列化**：`fromJson` 字段缺失或类型错误。
- **生命周期**：资源未释放、Controller 未正确初始化。

Agent 场景约定（`flutter-agent/workflow.md`）：修 Bug 时**先定位根因，修复后补充说明**，必要时补测试。

## 6. 性能优化 Workflow

性能优化需要先测量、再定位、后优化，避免凭感觉。

```text
明确性能目标（启动 / 滚动 / 构建）
  ↓
先用工具测量（DevTools / Profile）
  ↓
定位瓶颈（重建过多？大图？耗时操作在主线程？）
  ↓
针对性优化，构建验证并复测
```

常见优化手段：

- 用 `const` 构造器减少重建。
- 合理使用 `RepaintBoundary`、`ListView.builder` 懒加载。
- 大图压缩 / 缓存，避免布局抖动。
- 耗时操作移出主线程（`compute` / `isolate` / 后台任务）。

原则：**性能优化是测量驱动的**，AI 应基于测量数据给建议，而不是空谈。

## 7. 重构 Workflow

重构的前提是**先给方案、确认后再动手**（`flutter-agent` 场景约定）。

```text
识别重构目标（巨型 Widget、重复代码、分层混乱）
  ↓
AI 给出重构方案（拆什么、怎么拆、影响范围）
  ↓
人工确认方案
  ↓
小步重构，每步构建验证
  ↓
回归测试，确认行为不变
```

安全重构要点：

- 每次只做一类重构，避免大爆炸式改动。
- 优先行为不变：重构不改功能，只改结构。
- 依托测试保证行为可验证。
- 重构后跑 `flutter analyze` 与测试。

## 8. Code Review Workflow

AI 辅助 Code Review 能降低人工审查成本，但要与人工判断结合。

```text
提交变更
  ↓
AI 对照 rules/flutter 逐项审查
  ↓
输出结构化意见（命名 / 分层 / 错误处理 / 性能 / 安全）
  ↓
人工复核，采纳合理意见
```

审查维度：

- 命名是否规范、是否存在魔法数字 / 硬编码。
- 分层是否清晰、职责是否单一。
- 错误处理与异常路径是否完整。
- 是否复用统一封装（网络、路由、组件）。
- 性能隐患与安全风险（敏感信息硬编码等）。

`05-best-practice.md` 有更系统的审查清单。

## 9. Flutter Agent Workflow

`agents/flutter-agent/` 把上述通用流程固化为一套 Agent 专属工作流：

```text
需求 → 分析 → 设计 → 生成 → 自检 → 交付
```

### 9.1 需求分析

明确页面 / 功能目标，确认技术栈与架构约束，输出任务拆解清单。

### 9.2 设计

确定数据模型与接口、状态管理与路由，输出文件结构。

### 9.3 生成

按模块生成代码（controller / binding / view / model），复用统一封装。

### 9.4 自检

对照验收清单逐项检查：可编译、分层正确、命名统一、无硬编码、错误处理完整。

### 9.5 交付

说明新增 / 修改的文件，说明需执行的命令（如 `flutter pub get`），提示后续注意事项。

### 9.6 场景响应

| 任务 | Agent 动作 |
| --- | --- |
| 生成页面 | 按 `generate_page` prompt 流程执行 |
| 修 Bug | 定位根因 → 修复 → 补充测试 / 说明 |
| Review | 按 review 规范给出结构化意见 |
| 重构 | 先给方案，确认后再动手 |

## 10. Prompt + Rules + Agent Workflow

把前面所有流程汇总为统一执行链。它回答了"**Flutter + AI 到底怎么跑通一次开发**"。

```text
需求
  ↓
Prompt（把需求翻译成明确、带约束的任务）
  ↓
Rules（加载编码规范，作为生成与验收依据）
  ↓
Flutter Agent（进入执行闭环，理解架构后动手）
  ↓
代码（分模块产出，遵循分层）
  ↓
测试（analyze / build / 单元测试）
  ↓
Review（对照 Rules 结构化审查）
```

各环节的职责：

- **Prompt**：负责"意图"，把模糊需求变成可执行任务，用 `【】` 占位符复用模板。
- **Rules**：负责"纪律"，约束生成与验收标准，保证风格一致。
- **Flutter Agent**：负责"执行"，理解架构、拆解任务、生成 / 自检 / 交付。
- **Model**：负责"推理与生成"，是能力底座。
- **MCP / Tools**：负责"触达外部"，连接构建、设计稿、编辑器等。
- **测试与 Review**：负责"验收"，保证质量闭环。

这一条链就是 `01-overview.md` 中 Flutter Workflow 的可执行版本。把每一步都标准化、可复用，Flutter 开发的效率与质量就能从"碰运气"变成"可复制"。

## 总结

- 新功能、页面、API、状态管理、Bug、性能、重构、Review，每类任务都有明确可复现的 Workflow。
- 统一原则：**先读后写、先方案后动手、可验证、遵循 Rules**。
- 所有流程最终汇入 需求 → Prompt → Rules → Flutter Agent → 代码 → 测试 → Review 的统一链路。
- `agents/flutter-agent` 是这套工作流的资产化固化，一次配置、长期复用。

## 参考资料

- `agents/flutter-agent/workflow.md`：Agent 工作流程定义。
- `prompts/flutter/generate_page.md`：页面生成的 Prompt 模板。
- `rules/flutter/`：编码与审查约束。
- `docs/11-agent/04-workflow.md`：通用 Agent Workflow 方法论。
