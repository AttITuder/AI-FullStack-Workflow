# Flutter AI 开发 FAQ

> 常见问题速查，覆盖 Flutter / Dart / Widget / State / Architecture / AI 生成 / Prompt / Rules / Agent / MCP / Debug / UI 还原 / 性能 / 测试 / 重构 / 企业项目。按 `Q + ### A` 格式组织，方便快速检索。

---

## Q1

本章节是 Flutter 教程吗？和官方文档有什么区别？

### A

不是。本章节讲的是"如何在 AI FullStack Workflow 中工程化地组织 Flutter 开发"——如何用 Model、Prompt、Rules、Agent、MCP 提升效率与质量。官方文档讲"Flutter 怎么做"，本章讲"AI 怎么帮你把 Flutter 工程做好、怎么沉淀可复用的工作流"。

## Q2

Flutter 在 AI FullStack Workflow 中处于什么位置？

### A

位于全栈实战层（`docs/12-flutter/`），前面是基础认知、工具实操、MCP / Rules / Prompt / Agent，后面是企业研发。它是前面所有方法论在移动端技术栈上的**工程化集成与落地示范**。

## Q3

为什么说 Flutter 特别适合 AI 辅助开发？

### A

因为声明式 Widget 树结构清晰、单一代码库可用 `flutter analyze` / `build` 快速验证、生态规范统一、架构分层可选。这些特性让 AI 生成结果可预测、可自动验证、可被快速纠正。

## Q4

AI 生成 Flutter 代码，最常见的失败点是什么？

### A

结构不符项目分层、路由未注册、网络层散落新建实例、状态管理三态处理不完整、硬编码与未处理的异步错误。规避方法是提供充足 Context、用 Rules 约束、用验收清单核对。

## Q5

Dart 和 AI 协作时要注意什么？

### A

保持类型标注清晰、避免 `dynamic` 泛滥、正确使用 `async` / `await` 并处理错误、通过 `flutter analyze` 保证静态检查通过。类型越明确，AI 与人的理解越一致。

## Q6

StatelessWidget 和 StatefulWidget 什么时候用？

### A

纯展示、无内部可变状态用 `StatelessWidget`；有需要自己管理并随时间变化的状态时用 `StatefulWidget`。原则：能用无状态就不用有状态，减少不必要的重建。

## Q7

Widget 树过于庞大怎么办？

### A

拆分：把可复用的 UI 提取为独立 Widget、把页面业务逻辑移交给状态管理层。让 AI 辅助拆解巨型 Widget，但要先给方案、依托测试确认行为不变。

## Q8

Flutter 状态管理为什么这么难？

### A

因为"数据如何驱动界面、跨页面如何共享、异步如何更新"涉及多个维度。关键是先想清楚状态粒度与流转，再选合适方案，而不是一上来堆重型框架。

## Q9

状态管理方案怎么选？GetX、Bloc、Provider、Riverpod 用哪个？

### A

看项目规模与团队：`setState` 适合局部状态；`Provider` / `Riverpod` 适合中小项目；`GetX` 一体化、写起来快；`Bloc` 事件驱动、可测性强。**没有唯一答案**，按需选择（见 `02-architecture.md`）。

## Q10

AI 会默认推荐 GetX，一定要用它吗？

### A

不一定。本项目资产以 GetX 为示例（`flutter-agent`、`generate_page` 都基于 GetX），但能否采用取决于项目现状。AI 应给"与项目对齐"的方案，而非武断强推某一框架。

## Q11

Flutter 的架构分层一般怎么分？

### A

常见四层：UI 层（页面 / Widget）、状态管理层、数据层（Model / Repository / 存储）、网络层（API 封装）。模块按业务划分，模块内再按科技分层，职责单一、边界清晰。

## Q12

Agent 是怎么理解 Flutter 架构的？

### A

`flutter-agent` 通过"先读后写"理解：读取目录结构、`pubspec.yaml`、路由与既有模块，对齐系统提示词中的技术栈约束，再按 `workflow.md` 的 `分析 → 设计 → 生成 → 自检` 流程工作。

## Q13

大型 Flutter 项目和中小项目组织方式有何不同？

### A

大型项目通常需要更清晰的模块边界、统一路由与依赖注入、多 Package / Monorepo、完整测试体系，并要考虑多人协作减少冲突。架构是随规模演进的，不是开局就上最重型方案。

## Q14

AI 生成页面时最该给哪些信息？

### A

页面要素与交互、技术栈与架构约束、相关模块 Context、接口与跳转目标、输出要求与验收清单。信息越明确，产出越符合预期（可用 `prompts/flutter/generate_page.md`）。

## Q15

AI 生成的页面结构不符合项目分层怎么办？

### A

一是提供项目标准结构作为 Context（如 `generate_page.md` 的期望输出结构），二是用 Rules 约束分层，三是改后由 Review 对照验收清单核对。

## Q16

AI 生成了未注册的路由，页面打不开怎么办？

### A

把"路由必须正确注册"写入验收清单，Review 时重点核对。新增页面时应要求 AI 同时产出路由注册，构建后实际跳转验证。

## Q17

如何让 AI 生成的代码符合团队命名规范？

### A

在 `rules/flutter/` 沉淀命名规范，并让 AI 在生成前对齐既有模块命名。Prompt 中明确"遵循项目命名风格"，Review 阶段按规则逐项核对。

## Q18

AI 生成的代码能直接上线吗？

### A

不能。AI 产出需经过 `flutter analyze`、构建、测试与人工 Review。AI 是"提效工具"，质量认证据在团队流程：测试、审查、安全校验缺一不可。

## Q19

AI 生成代码最常见的错误处理缺失是什么？

### A

未处理的网络异常、超时、空值（null）没有兜底、异步错误没有 catch、用户侧没有可理解的错误提示。这些都要写进 Rules 与验收清单约束。

## Q20

AI 辅助 Debug 时，应该怎么提问？

### A

提供报错信息、可复现路径、相关代码与预期行为，让 AI "先定位根因再给方案"。避免只给片段的"盲猜"，`flutter-agent` 强制先定位根因再修复。

## Q21

遇到 FloatingActionButton 被遮挡这类布局问题，AI 能帮什么？

### A

AI 能分析约束层级与布局逻辑，指出是父容器约束、滚动容器还是坐标计算问题。给出定位思路与修复建议，再经构建验证。

## Q22

UI 还原场景，AI 怎么提高还原度？

### A

提供设计稿（可结合 MCP 读取 Figma）、具体尺寸、颜色、间距与交互要求作为 Context，让 AI 按描述生成 Widget，再做视觉比对调整。Context 越细还原度越高。

## Q23

MCP 在 Flutter 开发中能干什么？

### A

连接外部能力：Figma 读设计稿辅助 UI 还原、GitHub / Git 管理分支与变更、构建 / ADB 提供反馈、文档 / 接口作为生成 Context。让 AI 触达"真实世界"数据。

## Q24

MCP 使用有什么安全注意？

### A

密钥一律走环境变量，不写进仓库；按需启用避免信息过载；涉及敏感操作时谨慎授权。配置参考 `mcp/` 与 `docs/08-mcp/`。

## Q25

Prompt 在 Flutter 开发中的角色是什么？

### A

把需求翻译成明确、带约束的任务。好的 Prompt 含角色设定、技术栈约束、明确任务、输出要求与验收清单。用 `【】` 占位符复用模板（`prompts/flutter/`）。

## Q26

Rules 在 Flutter 开发中的角色是什么？

### A

作为 ID 与开发的"行为准则"，约束命名、分层、封装、错误处理等，保证产出风格一致。Rules 要可执行、可检查，并同步给 Prompt 与 Agent 引用。

## Q27

Flutter Agent 和直接问 Chat AI 有什么区别？

### A

Agent 把"身份、技术栈、工作原则、流程"固化成资产，一次加载长期复用，命令式委派即可；Chat AI 每次都要重新描述一切，且缺少流程纪律。生产力更稳定、更可复制。

## Q28

flutter-agent 会怎么处理"重构"这类高风险任务？

### A

按 `workflow.md` 的场景响应，重构走"先给方案、确认后再动手"，而不是直接大改。这符合安全原则，避免破坏性变更。

## Q29

AI 如何辅助性能优化？

### A

AI 基于测量数据（DevTools / Profile）定位瓶颈，再给优化方案（const、懒加载、RepaintBoundary、移出主线程等），改造后复测确认收益。性能优化是测量驱动的。

## Q30

AI 可以做哪些测试？怎么做？

### A

可生成单元测试（状态管理、Model 序列化、工具函数）与 Widget 测试（关键 UI 与交互），覆盖关键分支与加载 / 成功 / 失败三态。跑 `flutter test` 验证，测试是信任 AI 产出的前提。

## Q31

重构如何保证不破坏功能？

### A

坚持"只改结构不改行为"，小步迭代、每步 `flutter analyze` / 构建 / 测试验证，依托测试兜底。重构前先让 AI 出方案人工确认，控制影响范围。

## Q32

企业 Flutter 项目怎么落地 AI 工作流？

### A

沉淀统一资产（`prompts/flutter/`、`rules/flutter/`、`agents/flutter-agent/`、`mcp/`），建立 Code Review + 人工拍板、分层测试、安全合规红线的质量闭环，从个人提效逐步演进为稳定可复现的团队流水线。
