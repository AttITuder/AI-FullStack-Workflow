# Android 工程架构

> 本文讲解 Android 工程的典型分层与组织方式，以及 AI / Agent 如何理解并利用这套架构高效产出代码。它不绑定单一架构方案——MVVM、Clean Architecture、Compose 都只是选项，选择权在项目实际。

---

## 1. Android 应用整体架构

一个可维护的 Android 应用，通常由多层职责清晰的代码组成。分层的目标是**降低耦合、提高可测性、让 AI 与团队成员能在明确边界内工作**。

典型的分层视图：

```text
UI 层（Activity / Fragment / Compose）
   ↓
ViewModel / 状态管理（连接 UI 与数据）
   ↓
Domain 层（业务用例）
   ↓
Data 层（Repository / Model / 本地数据）
   ↓
网络层 + 本地数据（Retrofit / OkHttp / Room / DataStore）
```

> 注意：这是**常见**的分层参考，不是唯一标准。不同项目可以采用不同架构（MVVM、MVP、Clean Architecture 等）。只要分层清晰、职责单一、AI 能读懂其意图，不必照搬。

架构设计的根本问题是：**如何让每一行代码，既容易被人维护，也容易被 AI 正确理解与扩展。**

## 2. UI 层

UI 层负责把数据渲染成界面、接收用户交互。

### 2.1 Activity

Activity 是传统 Android UI 的载体，管理一个屏幕及生命周期。它不应承载过多业务逻辑——职责应下移给 ViewModel，避免"上帝 Activity"。

### 2.2 Fragment

Fragment 用于构建可复用的 UI 片段与多屏适配（如手机 / 平板的 Master-Detail）。Fragment 有独立的生命周期，AI 生成代码需正确处理其生命周期与 View 的销毁。

### 2.3 Compose

Jetpack Compose 是声明式 UI，用 Kotlin 描述界面：

```kotlin
@Composable
fun LoginScreen(viewModel: LoginViewModel) {
    val state by viewModel.uiState.collectAsState()
    Column {
        TextField(value = state.username, onValueChange = viewModel::onUsernameChange)
        Button(onClick = viewModel::login) { Text("登录") }
        if (state.error != null) { Text(state.error!!) }
    }
}
```

Compose 让 UI 结构清晰、可组合，是 AI 最"顺手"的 UI 形式。

### 2.4 AI 视角下的 UI 层

对 Agent 而言，UI 层是"目标最明确"的部分——需求往往描述的是界面。AI 生成代码时要确认项目用的是 XML + View 还是 Compose，并保持一致。

## 3. 状态管理

状态管理决定"数据变化如何驱动界面更新"。

### 3.1 ViewModel

ViewModel 是 Android 官方推荐的状态持有者，持有 `LiveData` / `StateFlow` 等可观察数据，并通过 `SavedStateHandle` 应对配置变更（旋转屏幕）。它只存活于其作用域，不直接持有 View 引用。

### 3.2 StateFlow

现代推荐的方案，用 `StateFlow<List<T>>` 等表达不可变状态，配合 `collectAsState()`（Compose）或 `observe`（传统 View）消费。类型安全、可组合、易测试。

### 3.3 LiveData

LiveData 是生命周期感知的可观察数据，适合与传统 View / Fragment 配合。与 StateFlow 相比，更依赖生命周期，边缘行为略多。

| 方案 | 特点 | 适用场景 |
| --- | --- | --- |
| ViewModel + LiveData | 生命周期感知，官方传统路径 | XML + View UI |
| ViewModel + StateFlow | 类型安全、可组合、易测试 | Compose 或追求现代方案 |
| 其他（如 MVI 框架） | 单向数据流、更高约束 | 复杂状态与严格分层 |

> 本项目不把某一状态管理方案设为唯一答案。选择依据是项目 UI 体系、规模与团队熟练度。

### 3.4 AI 视角下的状态管理

Agent 生成代码时需正确理解：加载 / 成功 / 失败三态如何表示，异步数据如何更新状态，配置变更如何保持状态。`03-development-workflow.md` 有专门的状态处理说明。

## 4. Domain 层

Domain 层封装**业务规则**，通过用例（Use Case）暴露业务操作，不依赖具体 UI 与数据实现。它让业务逻辑可独立测试、可复用。对中小项目可以省略，直接由 ViewModel 调用 Data 层——**分层应按需引入，而不是为了分层而分层**。

## 5. Data 层

Data 层负责提供与存储数据，向上供给 Domain / ViewModel。

### 5.1 Model

Model 是数据的结构化表示。AI 生成时要保证字段与接口 / 业务对应、序列化正确、类型合理（Kotlin 常用 `data class` + `kotlinx.serialization` 或 Gson / Moshi）。

### 5.2 数据来源抽象

Data 层通过 Repository 对外统一暴露，屏蔽"来自网络还是本地缓存"，让上层不关心数据来源，也让缓存策略可独立演进。这是 AI 重构与测试的重要边界。

## 6. Repository 模式

Repository 是数据层的门面，把网络与本地数据统一到一个入口：

```kotlin
interface UserRepository {
    suspend fun getUser(): User
}

class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao,
) : UserRepository {
    override suspend fun getUser(): User {
        return dao.getCache() ?: api.getUser().also { dao.save(it) }
    }
}
```

Repository 的价值：屏蔽数据来源、支持缓存策略、便于单元测试（mock Repository）。AI 在生成数据访问代码时，应尽量遵循 Repository 模式对齐项目约定。

## 7. 网络层

网络层负责与后端通信，并做统一封装。

### 7.1 Retrofit

Retrofit 是常见的类型安全 HTTP 客户端，通过接口声明 API：

```kotlin
interface UserApi {
    @GET("user/{id}")
    suspend fun getUser(@Path("id") id: Long): User
}
```

### 7.2 OkHttp

OkHttp 是底层 HTTP 引擎，配合 Retrofit 使用。在 `OkHttpClient` 上配置拦截器（Token 注入、日志、错误处理）与超时。

### 7.3 API Service

API Service 是网络层对外暴露的接口定义集合。AI 生成网络代码时应复用统一 Client 与拦截器，不散落新建实例。

### 7.4 错误处理

统一解析响应结构、映射错误码、处理网络异常与超时，给出用户可理解的提示。这是 AI 生成网络代码时最常遗漏的一环。

## 8. 本地数据

本地数据负责离线能力与缓存，应封装在 Data 层，避免在 UI 散落。

### 8.1 Room

Room 是官方 SQLite 封装，通过 `@Entity` / `@Dao` / `@Database` 声明式访问数据库。AI 生成时需要同时生成 Entity、Dao，并处理版本迁移。

### 8.2 SharedPreferences

轻量键值存储，简单但不适合大数据与频繁读写。常用于偏好设置。

### 8.3 DataStore

现代替代 SharedPreferences 的方案（Preferences DataStore / Proto DataStore），基于协程与 Flow，类型安全、异步。官方推荐新项目使用 DataStore。

| 方案 | 特点 | 适用场景 |
| --- | --- | --- |
| Room | 结构化、可查询 | 复杂业务数据 |
| SharedPreferences | 简单同步 | 少量偏好 |
| DataStore | 异步、类型安全 | 偏好 / 小型序列化数据 |

## 9. 多模块架构

多模块是把代码按边界拆分成独立 Gradle 模块，让大型项目可并行演进、可独立编译。

### 9.1 模块划分示例

```text
app/              —— 应用入口，聚合依赖
feature/login/    —— 业务模块（页面、ViewModel）
feature/home/     —— 业务模块
core/network/     —— 网络基础
core/data/        —— 数据与 Repository
core/designsystem/—— 通用 UI 组件
```

### 9.2 模块化带来的 AI 协作价值

- 模块边界清晰，Agent 可以在"一个模块"内安全生成与修改，不污染其他模块。
- 新增功能时，AI 能顺着模块结构定位"该动哪里、不该动哪里"。
- 依赖方向明确，重构时的风险面可控。
- 但也引入"模块依赖管理"复杂度，AI 需理解 `build.gradle` 的依赖声明。

## 10. Android 大型项目架构

大型项目在基础分层之上，还需考虑：

### 10.1 架构演进路径

不追求一步到位，随规模渐进：

```text
单 Activity / Fragment + 直接调用数据
   ↓
引入 ViewModel + Repository（职责分离）
   ↓
多模块 + 依赖注入（Hilt / Koin）
   ↓
Domain 层 / 用例 + 完整测试体系
```

### 10.2 大型项目的关注点

- **分层与依赖方向**：UI → Domain → Data，避免反向依赖。
- **可测性**：ViewModel、Repository、用例可被单元测试覆盖。
- **一致性**：命名、错误处理、依赖注入全局统一（这是 Rules 的作用）。
- **构建速度**：模块化 + 增量构建优化。
- **多人协作**：模块边界清晰，减少合并冲突。

### 10.3 AI 在架构演进中的角色

AI 可以分析现有组织方式、发现职责不清或依赖倒置、给出拆分建议、辅助迁移重构。但重大架构变更必须**先出方案让团队确认**，不能直接改动。

## 11. AI 辅助架构分析

AI 可以作为一个"架构顾问"来使用。

### 11.1 分析输入

给 AI 提供模块结构、`build.gradle`、关键代码、Manifest 等，让它输出：

- 当前架构的分层是否清晰。
- 职责是否单一，耦合是否过高。
- 潜在的重构点与新功能的最佳落点。

### 11.2 分析输出格式

结构化输出：**现状**（各层与依赖关系）、**问题**（职责不清、循环依赖等）、**建议**（按优先级给出可执行的重构步骤）、**不强制**（尊重项目现状）。

### 11.3 可信度控制

AI 的架构判断需人工校验。把 AI 当作"假设生成器"，由团队用真实代码验证——"AI 分析 → 人决策"。

## 12. Agent 如何理解 Android 项目结构

`agents/android-agent` 作为执行层，必须能"读懂"项目后再动手。

### 12.1 先读再写

Agent 在"分析"阶段就应理解：Gradle 结构与模块、UI 是 XML 还是 Compose、状态管理是 LiveData 还是 StateFlow、网络 / 数据层怎么组织、依赖注入方式。

### 12.2 对齐项目技术栈

Agent 生成代码时对齐项目既有方案（Kotlin / Java、MVVM / 其他、Retrofit / 其他），而不另起炉灶。通过 `rules/android/` 的规范进一步保证一致性。

### 12.3 场景化响应

Agent 针对不同任务采取不同动作：生成页面按模块结构与技术栈产出、修 Bug 先定位根因、重构先出方案确认。对不熟悉的架构"不瞎猜"，必要时询问人工。

### 12.4 帮助 Agent 理解的最佳实践

- 保持模块结构与命名一致。
- 用简洁注释说明模块职责。
- `rules/android/` 明确规范，给 Agent 可检查的约束。
- 大改动前，让 AI 先生成"理解后的改造方案"再做。

## 总结

- Android 架构核心是**分层清晰、职责单一**：UI / ViewModel / Domain / Data / 网络各司其职。
- **不绑定单一架构方案**——MVVM、Clean Architecture、Compose 都只是选项，取决于项目实际。
- Repository、多模块、依赖注入是大型项目组织与 AI 安全协作的基础。
- Agent 的价值在于**先理解项目结构再动手**，在明确边界内生成与修改代码。

## 参考资料

- `agents/android-agent/`：Android Agent 配置与职责。
- `prompts/android/`：Android 开发 Prompt 模板规划。
- `docs/09-rules/`：Rules 如何约束架构落地。
- `docs/12-flutter/02-architecture.md`：同层级 Flutter 架构章节（分层方法论相通）。
