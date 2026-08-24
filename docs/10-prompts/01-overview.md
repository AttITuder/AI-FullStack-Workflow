# Prompts 概述

> 定位：不是 Prompt 技巧合集，而是 AI FullStack Workflow 知识库。目标读者：Android 开发者、Flutter 开发者、全栈开发者、企业研发人员。

---

## 1. Prompt 是什么

### 定位

Prompt 是传递给 AI 的**任务意图载体**。它用自然语言描述这次任务的目标、背景、约束与验收标准，是人与 AI 协作时信息量最大的一次输入。

在本项目的语境下，需要区分四个相关概念：

| 概念 | 含义 |
| --- | --- |
| Prompt | 广义上一切发给 AI 的指令文本 |
| Prompt Engineering | 把"写 Prompt"从碰运气变成工程方法的设计学科 |
| Coding Prompt | 面向编码任务的 Prompt：生成、修复、重构、测试 |
| Agent Prompt | Agent 的系统提示词或任务委派词：定义角色与工作方式 |

四者的关系：Coding Prompt 与 Agent Prompt 是 Prompt 在 AI Coding 领域的两种主要形态；Prompt Engineering 是把两者写好的方法论。本章节讨论的是这套方法论及其在研发工作流中的组织方式。

### 一个关键认知

Prompt 不是"咒语"，而是**需求文档的最小化形态**。写得好的 Prompt 本质上回答了软件工程的基本问题：做什么（Goal）、基于什么现状（Context）、遵守什么限制（Constraints）、交付什么结果（Output）、怎么算完成（Validation）。这五问与人类工程师接需求时的问题完全一致。

## 2. 为什么 AI Coding 需要 Prompt

### 普通聊天与 AI Coding 的差异

普通聊天的链路很短：

```text
问题
   ↓
回答
```

问题模糊，答案也可以模糊，双方都能容忍。AI Coding 的链路长得多：

```text
需求
   ↓
理解
   ↓
规划
   ↓
执行
   ↓
验证
```

每一步都建立在前一步的基础上。而整条链路的起点是 Prompt——**任务意图是否清晰，决定了后面所有环节是否在正确的方向上**。

### 垃圾进，垃圾出

模型的理解能力再强，也无法凭空补全缺失的信息：

- 意图不清 → Agent 规划出错误的方向。
- 约束缺失 → 产出不符合团队规范。
- 验收标准缺位 → "完成了"没有客观定义，返工不可避免。

Prompt 质量是 AI Coding 产出质量的第一道闸门。这也是为什么本项目把它作为独立的知识体系来建设，而不是当成使用工具时的临时技巧。

### 从个人技巧到团队能力

同一个任务，不同成员写的 Prompt 质量参差不齐，产出自然不稳定。把好的 Prompt 沉淀为团队资产（`prompts/` 目录），新人复用即获得老手的经验——这是 Prompt 工程化的核心动机。

## 3. Prompt 在 AI FullStack Workflow 中的位置

在完整工作流中，Prompt 承担**任务意图层**的角色：

```text
Model（推理能力层）
   ↓
Prompt（任务意图层）
   ↓
Rules（行为约束层）
   ↓
Agent（执行层）
   ↓
MCP（工具能力层）
   ↓
Workflow（流程编排层）
   ↓
Code（最终产出）
```

六层各司其职：

- **Model** 决定 AI 有多聪明。
- **Prompt** 决定 AI 这次要干什么。
- **Rules** 决定干活时必须遵守什么。
- **Agent** 决定以什么角色和方式干。
- **MCP** 决定能触达哪些外部工具。
- **Workflow** 把以上能力编排成稳定可复现的流程。

一个类比：如果把一次 AI 协作看作一个工程项目——Model 是施工队的技术水平，Prompt 是本次的任务委托书，Rules 是施工规范，Agent 是项目经理，MCP 是设备与材料通道，Workflow 是施工组织方案。委托书写不清楚，再强的施工队也会返工。

在知识库层面，Prompts 章节承接 Rules 章节（`docs/09-rules/`），后接 Agent 章节（`docs/11-agent/`），完成"约束层 → 意图层 → 执行层"的衔接。

## 4. Prompt 与 Rules 的区别

这是最容易混淆的一对概念，核心区别一句话：

> **Prompt 告诉 AI"这次任务做什么"，Rules 告诉 AI"长期应该怎么做"。**

| 维度 | Prompt | Rules |
| --- | --- | --- |
| 生效范围 | 单次任务 | 长期、所有任务 |
| 内容重点 | 本次目标、上下文、输出要求 | 编码规范、行为边界、禁止事项 |
| 变更频率 | 每个任务不同 | 低频演进 |
| 存放位置 | 对话输入或 `prompts/` 模板 | `rules/` 或 Agent 配置 |

判断一条内容放哪里：对未来的每个任务都成立 → Rules；只针对当前这一次 → Prompt。

两者是配合而非替代关系。一个新功能任务的完整输入 = **Prompt（意图）+ Rules（纪律）+ Context（现状）**。Prompt 中不需要复述规则全文，引用即可："遵循 R-03 页面分层"。

## 5. Prompt 与 MCP 的区别

| 维度 | Prompt | MCP |
| --- | --- | --- |
| 属性 | 任务描述 | 能力通道 |
| 回答的问题 | 要做什么、做成什么样 | 能用什么工具、触达哪些数据 |
| 形态 | 自然语言文本 | Server + Tools 协议 |

分工示例：Prompt 写"修复登录页崩溃并通过真机验证"，描述的是任务；Agent 执行时通过 ADB MCP 安装 APK、拉取日志，通过 Gradle MCP 构建项目——这些是能力。**Prompt 描述任务和目标，MCP 提供达成目标的工具和数据。**

两者配合的关键点：写 Prompt 时可以假设工具可用（"构建并安装到设备验证"），不必描述工具细节；工具调用的纪律则由 Rules 约束（如"生产环境只读"）。

## 6. Prompt 与 Agent 的关系

Agent 是 Prompt 的**宿主与放大器**：

1. **Agent 自带系统级 Prompt**：定义角色与工作方式（如 flutter-agent 的专家设定与分层要求），对所有任务生效。这类 Prompt 长期不变，性质上接近 Rules。
2. **用户 Prompt 是任务级输入**：每次委派的具体指令，建立在系统 Prompt 提供的角色基础之上。

配合方式：

- 简单任务：直接对话，用户 Prompt 即全部输入。
- 高频任务：把成熟的用户 Prompt 沉淀到 `agents/` 对应 Agent 的配置中固化（参考 `agents/flutter-agent/` 的组织思路）。
- 复杂流程：一个 Prompt 启动 Agent，Agent 内部再按 workflow.md 的步骤拆解执行。

工程含义：**当某个 Prompt 反复被手写时，它就该升级为 Agent 配置的一部分了。**

## 7. Prompt 与 Context 的关系

Context 是模型可见的全部上下文：代码库内容、会话历史、Rules、Prompt 本身。Prompt 与 Context 的关系有两层：

### Prompt 是 Context 的一部分

你写的每个字都会进入模型上下文。因此 Prompt 要精炼——冗长的废话挤占真正有用的信息空间，稀释模型注意力。

### Prompt 可以指挥获取 Context

高阶用法是在 Prompt 中指示 AI 主动收集上下文：

```text
动手前先阅读 modules/login/ 下现有页面的分层实现，
遵循同样的结构。
```

这比把相关文件内容全部贴进 Prompt 更高效——让 Agent 用自己的检索能力按需加载，而不是人肉搬运上下文。

两者的边界：Context 回答"AI 看到了什么"，Prompt 回答"我们要求它做什么"。Prompt 中只放任务必需的信息，其余靠 Context 机制提供。

## 8. Prompt 的基本类型

按用途划分，AI Coding 中的常见 Prompt 类型：

| 类型 | 用途 | 典型场景 |
| --- | --- | --- |
| Task Prompt | 通用的任务委派 | 日常对话式协作 |
| Coding Prompt | 生成符合规范的代码 | 新页面、新模块开发 |
| Debug Prompt | 定位与修复问题 | 崩溃、构建失败 |
| Review Prompt | 结构化代码审查 | PR 审查、质量把关 |
| Refactor Prompt | 受控重构 | 结构优化、消除重复 |
| Test Prompt | 生成测试用例 | 单元测试、边界用例 |
| Architecture Prompt | 架构分析与设计 | 分层设计、技术选型 |
| Product Prompt | 产品侧辅助 | 需求拆解、方案评估 |

这些类型不是并列的模板清单，而是**同一套结构方法在不同场景下的应用**。类型对应的是本项目中 Prompt 的组织维度之一（见下节）。

## 9. 本项目中的 Prompt 资产

本项目将 Prompt 分为两个层面：

### Prompt 资产：`prompts/`

存放真正可复制、可使用的 Prompt 模板，按两个维度组织为十二个分类：

**技术栈维度**（面向特定语言 / 框架的开发任务）：

```text
prompts/
├── android/     # Android 开发 Prompt
├── dart/        # Dart 语言 Prompt
├── flutter/     # Flutter 开发 Prompt
├── java/        # Java 语言 Prompt
├── kotlin/      # Kotlin 语言 Prompt
└── vue/         # Vue 开发 Prompt
```

**场景维度**（跨技术栈的通用任务）：

```text
├── architecture/   # 架构设计类
├── product/        # 产品需求类
├── refactor/       # 重构类
├── review/         # 代码审查类
├── test/           # 测试类
└── ui/             # UI 相关类
```

目前资产处于持续建设状态：`prompts/flutter/generate_page.md` 是首个完整落地的 Prompt（Flutter 页面生成，含 Controller / Binding / View / Route 四件套生成）；flutter、android、review、architecture 分类已有明确的规划清单，其余分类待逐步填充。具体以 `prompts/` 各目录的实际内容为准。

### Prompt 规范

所有 Prompt 资产的编写遵循 `docs/00-roadmap/writing-style.md` 第 6 节：

- 每个 Prompt 独立成文，包含**适用场景、使用说明、期望输出、验收清单**。
- Prompt 放在围栏代码块内，语言标识用 `text`，便于直接复制。
- 占位符统一使用 `【】`，例如 `【页面名称】`。

### 知识体系：`docs/10-prompts/`

本章节不存放可复制的 Prompt 正文，而是回答方法论问题：

- Prompt 应该怎么设计？（`02-prompt-structure.md`）
- 有哪些成熟的 Prompt 模式？（`03-prompt-patterns.md`）
- Prompt 如何驱动真实研发流程？（`04-workflow.md`）
- 团队如何管理与维护 Prompt？（`05-best-practice.md`）

两者的关系类比：`docs/10-prompts/` 是"写作方法论"，`prompts/` 是"范文与模板库"。新增 Prompt 资产时，以本章节的方法论为准绳；本章节讲解时引用资产做案例分析，但不复制正文。

## 10. Prompt 的价值

### 提高任务成功率

意图清晰的任务一次通过率显著更高。结构化的 Prompt 让 Agent 在规划阶段就锁定正确方向，减少"生成的代码完全不对路"的返工。

### 降低沟通成本

成熟的 Prompt 模板让"说清楚一件事"变成填空题。团队成员不需要每次都从零组织语言，也不需要在对话中反复澄清需求。

### 提高输出一致性

同一份 Prompt 在不同成员手中产出风格一致的代码。Prompt 与 Rules 配合后，一致性从"依赖个人表达习惯"变成"由资产保证"。

### 提高复用能力

一次打磨、处处受益。验证过的 Prompt 直接复制到同类任务，微调占位符即可交付——这是 Prompt 相对于一次性对话的根本优势。

### 沉淀团队知识

每个高质量的 Prompt 都封装着一段团队经验：这个场景容易踩什么坑、必须交代什么约束、验收看哪几条。Prompt 库是团队的"活的经验库"，新人复用即继承。

## 11. 本章节学习路线

围绕 Prompt 工程化的进阶顺序：

1. **概述（本篇）**：理解定位、与相邻层的边界。
2. **结构设计**：掌握好 Prompt 的八要素（`02-prompt-structure.md`）。
3. **Patterns**：学习场景 → 模式 → 效果的匹配方法（`03-prompt-patterns.md`）。
4. **Workflow**：把 Prompt 组织进真实研发流程（`04-workflow.md`）。
5. **最佳实践**：团队级 Prompt 资产管理（`05-best-practice.md`）。
6. **FAQ**：常见问题与排查（`06-faq.md`）。

建议配套阅读：

- `prompts/flutter/generate_page.md`：对照学习标准 Prompt 资产的结构。
- `docs/09-rules/01-overview.md` 第 4 节：Prompt 与 Rules 分工的另一视角。
- `rules/` 目录：理解 Prompt 引用的约束来源。

## 总结

- Prompt 是任务意图层：描述这次做什么、基于什么现状、遵守什么限制、交付什么结果。
- AI Coding 链路长，Prompt 质量决定整条链路方向，是产出质量的第一道闸门。
- 六层分工：Model 定能力，Prompt 定意图，Rules 定纪律，Agent 定执行，MCP 定连接，Workflow 定流程。
- Prompt 与 Rules 区分标准：单次任务的要求写 Prompt，长期成立的约束进 Rules。
- Prompt 类型是同一套方法在不同场景的应用，对应 `prompts/` 的组织维度。
- 双层结构：`prompts/` 存可复用资产，`docs/10-prompts/` 存方法论。
- 价值内核：成功率、沟通成本、一致性、复用、团队知识沉淀。

## 参考资料

- 下一节：`02-prompt-structure.md`
- Prompt 资产：`prompts/` 目录
- 写作规范中的 Prompt 规范：`docs/00-roadmap/writing-style.md`
