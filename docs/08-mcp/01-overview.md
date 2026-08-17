# MCP 概述

> 定位：不是 MCP 协议教程，而是 AI FullStack Workflow 知识库。目标读者：Android 开发者、Flutter 开发者、全栈开发者、企业研发人员。

---

## 1. MCP 是什么

### 定位

MCP（Model Context Protocol）是一个**开放标准协议**，用于连接 AI 模型与外部工具、数据源和系统。它的核心定位是：让 AI Agent 以标准化方式访问企业内外部能力，而不需要为每个工具编写定制集成代码。

它不是另一个 AI 编码工具，也不是替代 Coding Agent 的产品，而是**让 Agent 能力延伸到真实工具与数据的基础设施**。

### 解决的问题

MCP 主要解决三类问题：

1. **工具集成碎片化**：每个 AI 工具对接外部系统都要写定制代码，维护成本高、一致性差。MCP 提供统一协议，一次实现、多处复用。
2. **能力边界受限**：Agent 只能操作本地文件和终端，无法触达 GitHub、数据库、项目管理、设计稿等外部系统。MCP 打通了 Agent 与外部能力的连接。
3. **企业工具链协同**：企业研发涉及多个工具平台，AI 需要同时访问代码仓库、需求管理、设计系统、文档平台。MCP 让 Agent 成为连接这些系统的"中枢"。

### 与传统 API 的区别

| 维度 | 传统 API | MCP |
| --- | --- | --- |
| 集成方式 | 每个工具单独对接 | 统一协议，标准化接入 |
| 调用方 | 应用程序 | AI Agent |
| 发现机制 | 人工查阅文档 | Agent 自动发现可用工具 |
| 上下文管理 | 人工传递 | 协议内置上下文管理 |
| 适用场景 | 应用间通信 | AI 与外部系统连接 |

### 与 AI Agent 的关系

MCP 是 Agent 能力的**延伸层**。Agent 负责理解需求、规划任务、生成代码；MCP 让 Agent 能触达代码之外的真实工具与数据。没有 MCP，Agent 只能操作本地文件；有了 MCP，Agent 可以访问 GitHub、查询数据库、读取设计稿、更新项目管理工具。

## 2. 为什么需要 MCP

### AI 能力演进路径

AI 在软件开发中的角色经历了清晰的演进：

```text
模型（Model）
   ↓
Coding Agent
   ↓
Tool Calling
   ↓
外部工具连接（MCP）
```

1. **模型阶段**：AI 只能生成文本，开发者手动复制粘贴结果。
2. **Coding Agent 阶段**：AI 能读写本地文件、执行终端命令，但能力边界限于本机环境。
3. **Tool Calling 阶段**：AI 能调用预定义的函数/工具，但每个工具需要定制集成。
4. **MCP 阶段**：标准化协议让 Agent 以统一方式发现和调用任意 MCP 兼容工具。

### 从本地到企业

当 AI Coding Agent 从个人工具走向企业级应用时，必须解决：

- 如何安全地连接企业内部系统？
- 如何让 Agent 访问项目管理、设计、文档等平台？
- 如何在不修改 Agent 核心的前提下扩展能力？

MCP 正是为解决这些问题而生的协议层。

## 3. MCP 核心概念

### MCP Host

MCP Host 是**发起 MCP 连接的应用程序**。它负责管理 Client 与 Server 之间的连接生命周期。在本项目中，AI Coding Agent（如 OpenCode、Codex、Claude Code）就是 MCP Host。

### MCP Client

MCP Client 是 Host 内部的**协议客户端**，负责与 MCP Server 建立连接、发送请求、接收响应。每个 Client 通常对应一个 MCP Server 连接。

### MCP Server

MCP Server 是**提供具体工具能力的服务端程序**。它封装了对外部系统的访问逻辑，通过 MCP 协议暴露 Tools、Resources 和 Prompts。例如：GitHub MCP Server 封装了 GitHub API，让 Agent 能操作仓库、Issues、PR。

### Tools

Tools 是 MCP Server 暴露的**可执行能力单元**。Agent 可以发现并调用这些工具来完成特定任务。例如：`create_issue`、`query_database`、`read_design_file`。

### Resources

Resources 是 MCP Server 提供的**只读数据源**。Agent 可以读取但不修改。例如：项目配置文件、数据库 schema、API 文档。

### Prompts

Prompts 是 MCP Server 预定义的**提示词模板**。它们为特定场景提供结构化的输入模板，帮助 Agent 更准确地完成任务。

### Context

Context 是 Agent 在调用 MCP 工具时的**上下文信息**。包括当前任务、项目状态、历史对话等。MCP 协议确保上下文在 Host、Client、Server 之间正确传递。

## 4. MCP 在 AI FullStack Workflow 中的位置

在完整工作流中，MCP 承担**工具连接层**的角色：

```text
需求
   ↓
AI Model（理解与推理）
   ↓
Agent（规划与执行）
   ↓
MCP（工具发现与调用）
   ↓
企业工具 / 数据 / 开发环境
   ↓
执行结果
   ↓
验证与反馈
```

- **AI Model**：提供理解与推理能力。
- **Agent**：规划任务步骤，决定需要哪些外部工具。
- **MCP**：发现可用工具，建立连接，传递参数与结果。
- **企业工具**：GitHub、Jira、Figma、数据库等实际系统。
- **验证与反馈**：Agent 根据工具返回结果继续工作或调整方案。

在知识库层面，MCP 章节处于 Milestone 3（AI Coding 工具）与 Milestone 4（企业研发流程）的衔接位置，是连接 Agent 能力与企业工具链的桥梁。

## 5. MCP 与传统 API / Plugin / Function Calling 的区别

### 与传统 API 的区别

传统 API 是应用之间的通信接口，由开发者编写调用代码。MCP 是 AI Agent 与外部系统的连接协议，由 Agent 自动发现和调用。

- **传统 API**：人写代码调用，需要查阅文档、处理认证、编写集成层。
- **MCP**：Agent 自动发现工具、理解参数、发起调用，人只需要授权和验收。

### 与 Plugin 的区别

Plugin 是特定平台的扩展机制，通常与宿主应用强绑定。MCP 是跨平台的开放协议，任何兼容 MCP 的 Host 都能使用同一个 MCP Server。

- **Plugin**：绑定特定宿主（如 VS Code Plugin 只能在 VS Code 用）。
- **MCP**：跨 Host 复用，一个 GitHub MCP Server 可以同时被 OpenCode、Codex、Claude Code 使用。

### 与 Function Calling 的区别

Function Calling 是模型层面的能力，让模型输出结构化的函数调用请求。MCP 是协议层面的标准，定义了工具如何被发现、连接、调用和返回结果。

- **Function Calling**：模型决定"调用什么函数、传什么参数"。
- **MCP**：定义"工具在哪里、怎么连接、怎么调用、怎么返回"。

Function Calling 是 MCP 的基础能力之一，MCP 在其之上增加了工具发现、连接管理、上下文传递、安全控制等企业级能力。

## 6. MCP 与 OpenCode / Codex / Claude Code / CodeBuddy / Pi Agent 的关系

**核心关系：MCP 不是 Coding Agent 的竞争产品，而是连接 Agent 与外部能力的协议/机制。**

| Agent | 与 MCP 的关系 |
| --- | --- |
| OpenCode | 作为 MCP Host，通过 MCP 连接外部工具，扩展本地 Agent 的能力边界 |
| Codex | 云端 Agent，通过 MCP 连接企业工具链，实现云端任务与外部系统协同 |
| Claude Code | 终端 Agent，通过 MCP 扩展工具调用能力，增强本地开发体验 |
| CodeBuddy | 集成式助手，通过 MCP 接入本土化工具与服务 |
| Pi Agent | 轻量智能体，通过 MCP 获得外部系统访问能力 |

所有 Agent 共享同一套 MCP 生态：一个 GitHub MCP Server 可以同时服务于多个 Agent，团队不需要为每个 Agent 重复开发集成代码。

## 7. 本项目中的 MCP

本项目在 `mcp/` 目录中规划了以下 MCP Server：

### 开发工具

- **adb** — Android 调试桥，连接 Android 设备进行调试、日志分析、APK 安装
- **git** — Git 版本控制，操作本地仓库、分支、提交历史
- **gradle** — Gradle 构建工具，执行 Android 项目构建、依赖管理

### 代码平台

- **github** — GitHub 代码托管平台，操作仓库、Issues、Pull Requests、代码审查

### 项目管理

- **jira** — Jira 项目管理，管理需求、任务、Bug、Sprint

### 文档

- **confluence** — Confluence 企业知识库，读写团队文档、技术方案
- **showdoc** — ShowDoc 文档平台，管理 API 文档、项目文档

### 设计

- **figma** — Figma 设计工具，读取设计稿、获取设计规范、提取样式信息

### 数据库

- **mysql** — MySQL 数据库，查询与分析数据、查看表结构
- **sqlite** — SQLite 数据库，本地轻量数据库操作

这些 MCP Server 覆盖了企业研发流程中的核心工具链，是 AI FullStack Workflow 中 Agent 连接外部系统的基础能力。

## 8. MCP 的适用场景

### 代码开发

通过 Git、GitHub MCP，Agent 能管理代码版本、创建分支、提交改动、发起 PR，实现从编码到代码管理的完整闭环。

### 项目管理

通过 Jira MCP，Agent 能读取需求、更新任务状态、记录工作日志，让 AI 参与项目管理流程而非仅停留在编码环节。

### UI 设计

通过 Figma MCP，Agent 能读取设计稿、提取样式信息、理解页面结构，让 AI 生成的 UI 代码更贴合设计意图。

### 文档

通过 Confluence、ShowDoc MCP，Agent 能读取技术文档、更新 API 文档、生成开发文档，保持文档与代码同步。

### 数据库

通过 MySQL、SQLite MCP，Agent 能查询数据、分析 schema、辅助编写数据层代码，理解真实数据结构。

### Android 调试

通过 ADB MCP，Agent 能安装 APK、查看日志、分析设备状态，让 AI 参与移动端调试环节。

### Git 与 CI/CD

通过 Git、GitHub MCP，Agent 能操作版本控制、触发构建、查看 CI 状态，覆盖从代码提交到持续集成的流程。

### 企业内部系统

MCP 可以接入企业内部的各类系统（审批流、知识库、监控平台），让 AI Agent 成为企业研发流程的智能中枢。

## 9. MCP 的限制和风险

### 权限控制

MCP Server 暴露的工具可能包含写操作（创建 Issue、修改代码、更新数据库）。必须严格控制权限边界，避免 Agent 执行超出预期的操作。

### 安全风险

MCP 连接涉及认证凭证（API Key、Token、密码）。凭证泄露可能导致数据泄露或系统被攻击。必须使用安全的凭证管理方式。

### 数据访问

MCP Server 可以访问企业内部数据。数据的敏感度、合规要求、访问范围需要在接入前评估清楚。

### 工具调用风险

Agent 可能误用工具（错误参数、错误时机、错误目标）。需要在 MCP 层面设置调用限制，在 Agent 层面设置人工确认机制。

### 上下文污染

MCP 返回的数据进入 Agent 上下文后，可能影响后续决策。过大的上下文或无关数据会降低 Agent 表现。

### 企业网络

企业网络可能限制外部连接、要求代理配置、设置防火墙规则。MCP Server 的网络访问需要符合企业网络安全策略。

### 凭证管理

每个 MCP Server 通常需要独立的认证凭证。团队需要统一管理这些凭证，定期轮换，确保安全。

## 10. 本章节学习路线

围绕 MCP 的进阶顺序：

1. **概述（本篇）**：理解 MCP 定位、核心概念与适用场景。
2. **安装**：搭建 MCP 运行环境（`02-install.md`）。
3. **Server 与 Tools**：理解 MCP 架构与工具体系（`03-server-and-tools.md`）。
4. **Workflow**：建立 MCP 驱动的开发流程（`04-workflow.md`）。
5. **最佳实践**：沉淀企业级 MCP 使用规范（`05-best-practice.md`）。
6. **FAQ**：常见问题与排查（`06-faq.md`）。

## 总结

- MCP 是连接 AI Agent 与外部工具、数据和系统的标准协议，不是另一个 AI 编码工具。
- 核心概念包括 Host、Client、Server、Tools、Resources、Prompts。
- MCP 让 Agent 从"只能操作本地文件"升级为"能触达企业全工具链"。
- 本项目规划了 ADB、Git、GitHub、Gradle、Jira、Figma、Confluence、ShowDoc、MySQL、SQLite 共 10 个 MCP Server。
- MCP 的关键是安全与可控：权限最小化、凭证安全、上下文管理。

## 参考资料

- 下一节：`02-install.md`
- MCP Server 配置：`mcp/` 目录
