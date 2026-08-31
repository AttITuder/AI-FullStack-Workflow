# Android AI 开发 FAQ

> 常见问题速查，覆盖 Android / Kotlin / Java / Compose / XML / Gradle / Architecture / MVVM / Multi Module / AI 生成 / Prompt / Rules / Agent / MCP / Debug / Crash / 性能 / 测试 / 发布 / 兼容性 / 企业项目。按 `Q + ### A` 格式组织，方便快速检索。

---

## Q1

本章节是 Android 教程吗？和官方开发文档有什么区别？

### A

不是。本章节讲的是"如何在 AI FullStack Workflow 中工程化地组织 Android 开发"——如何用 Model、Prompt、Rules、Agent、MCP 提升效率与质量。官方文档讲"Android 怎么做"，本章讲"AI 怎么帮你把 Android 工程做好、怎么沉淀可复用工作流"。

## Q2

Android 在 AI FullStack Workflow 中处于什么位置？

### A

位于全栈实战层（`docs/13-android/`），前面是基础认知、工具实操、MCP / Rules / Prompt / Agent，同层级还有 Flutter、Vue，后面是企业研发。它是前面所有方法论在 Android 技术栈上的**工程化集成与落地示范**。

## Q3

AI 生成 Android 代码，最常见的失败点是什么？

### A

UI 体系用错（XML / Compose 混用）、主线程耗时操作、未处理空值与异步异常、网络层散落新建实例、生命周期与资源处理不当。规避方法：提供充足 Context、用 Rules 约束、用验收清单核对、构建与测试验证。

## Q4

Kotlin 和 AI 协作时要注意什么？

### A

善用 `data class`、`val`、空安全，谨慎使用 `!!`；用协程表达异步并正确处理调度器；遵循官方风格与 Lint。类型越明确、空安全处理越规范，AI 与人的理解越一致。

## Q5

AI 能维护存量 Java 代码吗？

### A

能。AI 能理解、重构 Java 代码，也可辅助渐进迁移到 Kotlin。关键是尊重既有 Java 风格、保持行为不变，混编项目注意类型交互（`@JvmStatic`、可空性标注），依托测试兜底。

## Q6

什么时候用 Activity，什么时候用 Fragment？

### A

Activity 管理一个屏幕与生命周期；Fragment 用于可复用 UI 片段与多屏适配（手机 / 平板）。都用时 Fragment 有独立生命周期，AI 生成代码需正确处理其生命周期与 View 销毁。

## Q7

Jetpack Compose 和 XML + View 怎么选？

### A

看项目现状：新项目或现代化项目可优先 Compose；存量项目多在 XML + View。AI 生成代码时须与实际项目一致，不能混用。**不把 Compose 设为唯一方案**。

## Q8

Compose 与 XML 相比，对 AI 更友好吗？

### A

通常更友好。Compose 是声明式、可组合的 Kotlin 描述，结构清晰、可预测，AI 更容易按描述产出一致结果。XML 布局则需注意层级与约束。

## Q9

XML 布局怎么避免过度嵌套？

### A

控制层级深度、用 `ConstraintLayout` 合理组织约束、避免不必要的嵌套容器。AI 生成后检查 lint 与层级，必要时简化。

## Q10

Android 状态管理用什么？LiveData 还是 StateFlow？

### A

传统 XML + View 常用 `ViewModel + LiveData`（生命周期感知）；现代方案常用 `ViewModel + StateFlow`（类型安全、可组合、易测试，配合 Compose 的 `collectAsState()`）。按 UI 体系与团队选择。

## Q11

ViewModel 在 AI 生成代码时为什么重要？

### A

ViewModel 是官方的状态持有者，持有 `LiveData` / `StateFlow`，通过 `SavedStateHandle` 应对配置变更（旋转屏幕）。它避免"上帝 Activity"把业务逻辑塞进 UI，让状态可测、可复用。

## Q12

Android 的 Domain 层必须要有吗？

### A

不一定。Domain 层封装业务用例，让业务逻辑独立可测、可复用，适合复杂业务。中小项目可直接由 ViewModel 调用 Data 层。**分层应接需引入**，而不是为了分层而分层。

## Q13

Repository 模式解决了什么问题？

### A

把网络与本地数据统一到"门面"，屏蔽数据来源、支持缓存策略、便于测试（mock Repository）。AI 生成数据访问代码时应尽量遵循 Repository 对齐项目约定。

## Q14

Android 网络层一般怎么组织？

### A

Retrofit（类型安全接口声明）+ OkHttp（底层 HTTP + 拦截器 / 超时）+ API Service（接口定义集合）。统一封装 Token 注入、日志、错误处理，AI 生成时应复用统一 Client，不散落新建实例。

## Q15

本地存储用 Room、SharedPreferences 还是 DataStore？

### A

Room 适合结构化业务数据；SharedPreferences 适合少量同步偏好（官方正逐步替代）；DataStore 异步、类型安全，是共享偏好与小型序列化数据的现代选择。按场景选择。

## Q16

AI 生成数据库代码要注意什么？

### A

Entity 字段与业务对应并注意索引，Dao 考虑数据变化通知（Flow / LiveData），数据库版本升级时正确编写 Migration 避免数据丢失，数据访问封装在 Data 层而非 UI。

## Q17

多模块架构对 AI 有哪些好处？

### A

模块边界清晰，AI 能在单个模块内安全生成与修改，不污染其他模块；能顺着"app → feature → core"定位改动落点；依赖方向明确，重构风险可控。代价是需理解 `build.gradle` 依赖。

## Q18

AI 会不会把多模块里原本该在 A 模块的代码生成到 B 模块？

### A

有可能。规避方法是提供模块结构与职责说明作为 Context，让 AI"先理解模块归属再写"，并在 Review 时检查代码落在正确模块。

## Q19

MVVM 是 Android 唯一推荐的架构吗？

### A

不是。MVVM 是 Android 官方推荐路径之一，但 MVP、Clean Architecture 等也常用于不同项目。**不同项目可以采用不同架构**，AI 应给出"与现状对齐"的方案，而不是武断强推。

## Q20

AI 生成架构代码时，会不会默认推 Clean Architecture？

### A

可能。AI 有偏好全域流行方案的倾向。应通过 Project Rules 与 Prompt 把"本项目架构"作为硬约束，并在 Review 时校验分层是否符合项目实际，避免过度设计。

## Q21

AI 怎么理解整个 Android 项目的结构？

### A

通过读取模块结构、`build.gradle`、Manifest、关键代码，识别 UI 体系（XML / Compose）、状态（LiveData / StateFlow）、网络、数据层，再对齐技术栈与架构，避免"盲写"。

## Q22

AI 生成代码后，最快的验证手段是什么？

### A

跑 `./gradlew assembleDebug` 验证编译、`./gradlew lint` 做静态检查、`./gradlew test` 跑单元测试。构建是 AI 生成后最先暴露问题的反馈闭环。

## Q23

遇到 Logcat 报错，AI 能帮什么？

### A

AI 结合 Logcat、堆栈与代码缩小范围：区分空指针 / 类型转换 / 线程 / 生命周期等问题，给出根因假设与修复建议。提供完整日志与相关代码能显著提高定位准确性。

## Q24

Crash 分析时，AI 怎么看堆栈？

### A

AI 从堆栈定位崩溃点（类 / 方法 / 行号），区分 Java / Kotlin 异常、ANR、native signal，结合版本 / 设备情报判断是否与兼容性或混淆相关，再结合 Logcat 确认根因并给修复方向。

## Q25

UI 还原场景，AI 怎么提高还原度？

### A

提供设计稿（可结合 MCP 读取 Figma）、具体尺寸、颜色、间距与交互要求作为 Context，让 AI 按描述生成 UI（Compose 或 XML），再做视觉比对调整。Context 越细还原度越高。

## Q26

MCP 在 Android 开发中能干什么？

### A

连接外部能力：ADB 连接设备 / 模拟器运行与调试、Gradle 提供构建反馈、GitHub / Git 管理变更、Figma 读设计稿辅助还原、文档 / 接口作为生成 Context，让 AI 触达"真实世界"数据。

## Q27

MCP 使用有什么安全注意？

### A

密钥一律走环境变量，不写进仓库；按需启用避免信息过载；涉及敏感操作时谨慎授权。配置参考 `mcp/` 与 `docs/08-mcp/`。

## Q28

Prompt 在 Android 开发中的角色是什么？

### A

把需求翻译成明确、带约束的任务。好的 Prompt 含角色设定、技术栈约束、明确任务、输出要求与验收清单，用 `【】` 占位符复用模板（`prompts/android/`）。

## Q29

Rules 在 Android 开发中的角色是什么？

### A

作为 ID 与开发的"行为准则"，约束 Kotlin / Java 风格、分层、网络 / 数据封装、线程与生命周期，保证产出风格一致、质量可预期。Rules 要可执行、可检查，并同步给 Prompt 与 Agent 引用。

## Q30

Android Agent 和直接问 Chat AI 有什么区别？

### A

Agent 把"身份、技术栈、工作原则、流程"固化成资产，一次加载长期复用，命令式委派即可；Chat AI 每次都要重新描述一切，且缺少流程纪律。生产更稳定、更可复制。

## Q31

AI 如何辅助 Android 性能优化？

### A

AI 基于测量数据（Profiler / Baseline）定位瓶颈（主线程阻塞、内存泄漏、布局过深、图片过大），再给优化方案，改造后复测确认收益。性能优化是测量驱动的。

## Q32

AI 可以做哪些 Android 测试？

### A

可生成单元测试（ViewModel 三态、Repository 缓存、Model 逻辑）与 Instrumented 测试（依赖 Android 环境的集成 / UI 测试）。跑 `./gradlew test` 验证，测试是信任 AI 产出的前提。

## Q33

AI 生成代码时的兼容性问题怎么控制？

### A

通过 Rules 约束 minSdk / targetSdk、提供项目约束到 Prompt、运行时注意版本判断与厂商差异。兼容性是人工经验密集区，AI 建议需结合真实设备验证，不能凭空覆盖。

## Q34

企业 Android 项目怎么落地 AI 工作流？

### A

沉淀统一资产（`prompts/android/`、`rules/android/`、`agents/android-agent/`、`mcp/`），建立构建（Gradle）+ 测试 + Code Review + 人工拍板 + 发布（签名 / 混淆 / 合规）的质量闭环，从个人提效逐步演进为稳定可复现的团队流水线。
