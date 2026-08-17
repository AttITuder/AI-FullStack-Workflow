# MCP Server 与 Tools

> MCP 的核心基础章节。理解 Server、Client、Host、Tool、Resource、Prompt 的关系，以及本项目的 MCP Server 分类。

---

## 为什么需要了解

MCP 的核心价值在于 Server 暴露的 Tools。Agent 通过调用 Tools 完成真实任务，而不是停留在"生成文本"。理解 MCP 架构中的每个角色，才能正确设计、配置和使用 MCP。

## 1. MCP Server 是什么

MCP Server 是一个**独立运行的服务端程序**，封装了对外部系统的访问逻辑，通过 MCP 协议暴露能力。

### 核心职责

- **封装外部系统**：把 GitHub API、MySQL 查询、ADB 命令等封装成标准化的工具接口。
- **暴露 Tools**：声明自己能做什么，让 Agent 自动发现。
- **处理请求**：接收 Agent 的工具调用请求，执行对应操作，返回结果。
- **管理连接**：处理认证、网络、超时等连接层面的问题。

### 生命周期

```text
Host 启动
   ↓
Host 根据配置启动/连接 Server
   ↓
Server 初始化，注册工具列表
   ↓
Agent 发现可用工具
   ↓
Agent 按需调用工具
   ↓
Server 执行并返回结果
   ↓
Host 关闭时断开连接
```

### 两种运行模式

- **stdio 模式**：Server 作为 Host 的子进程运行，通过标准输入输出通信。适合本地 MCP Server。
- **HTTP/SSE 模式**：Server 作为独立服务运行，通过网络通信。适合远程 MCP Server。

## 2. MCP Client 是什么

MCP Client 是 Host 内部的**协议客户端组件**，负责与 MCP Server 建立和维护连接。

### 核心职责

- **建立连接**：按照 MCP 协议与 Server 握手，协商能力。
- **发送请求**：将 Agent 的工具调用请求转化为 MCP 协议消息。
- **接收响应**：接收 Server 返回的执行结果，传递给 Agent。
- **管理会话**：维护连接状态、处理重连、管理超时。

### 一个 Host 可以有多个 Client

每个 Client 对应一个 MCP Server 连接。例如 OpenCode 同时连接 GitHub、Jira、MySQL 三个 MCP Server 时，内部会有三个 Client 分别负责与各自的 Server 通信。

## 3. MCP Host 是什么

MCP Host 是**发起和管理 MCP 连接的应用程序**。在 AI FullStack Workflow 中，MCP Host 就是 AI Coding Agent。

### 核心职责

- **加载配置**：读取 MCP Server 配置，决定连接哪些 Server。
- **管理 Client**：为每个 Server 创建 Client，管理连接生命周期。
- **工具发现**：获取每个 Server 暴露的工具列表，供 Agent 使用。
- **调度调用**：Agent 决定调用哪个工具时，Host 将请求路由到对应的 Client。
- **结果整合**：将工具返回结果整合到 Agent 的上下文中。

### 本项目中的 Host

| Host | MCP 支持方式 |
| --- | --- |
| OpenCode | 通过配置文件声明 MCP Server，自动管理连接 |
| Codex | 云端 Agent，通过配置接入 MCP Server |
| Claude Code | 终端 Agent，通过配置文件声明 MCP Server |
| CodeBuddy | 集成式助手，内置 MCP 支持 |
| Pi Agent | 轻量智能体，通过配置接入 MCP Server |

## 4. Tool 是什么

Tool 是 MCP Server 暴露的**可执行能力单元**，是 Agent 与外部系统交互的核心接口。

### Tool 的组成

每个 Tool 包含：

- **名称**：唯一标识，如 `create_issue`、`query_database`。
- **描述**：说明工具的用途，帮助 Agent 理解何时使用。
- **参数定义**：输入参数的名称、类型、是否必填、说明。
- **返回结果**：执行后的输出格式。

### Tool 的分类

- **只读工具**：查询、搜索、读取（如 `list_issues`、`query_data`）。
- **写入工具**：创建、修改、删除（如 `create_issue`、`update_task`）。
- **执行工具**：运行命令、触发构建（如 `run_gradle`、`install_apk`）。

### Agent 如何使用 Tool

1. Agent 发现可用工具列表。
2. Agent 根据任务需要选择合适的工具。
3. Agent 构造工具调用参数。
4. Host 将请求路由到对应的 Client → Server。
5. Server 执行工具并返回结果。
6. Agent 根据结果继续工作。

## 5. Resource 是什么

Resource 是 MCP Server 提供的**只读数据源**，Agent 可以读取但不修改。

### 与 Tool 的区别

| 维度 | Tool | Resource |
| --- | --- | --- |
| 操作 | 可读可写 | 只读 |
| 用途 | 执行操作 | 提供上下文数据 |
| 示例 | `create_issue` | `project_config` |

### 典型用途

- 项目配置文件（让 Agent 了解项目结构）。
- 数据库 schema（让 Agent 了解数据模型）。
- API 文档（让 Agent 了解接口规范）。

### 使用方式

Agent 可以在执行任务前读取 Resource，获取必要的上下文信息，而不是依赖人工提供。

## 6. Prompt 是什么

Prompt 是 MCP Server 预定义的**提示词模板**，为特定场景提供结构化的输入模板。

### 典型用途

- 代码审查场景的结构化提示。
- 数据库查询的参数化模板。
- 文档生成的标准格式。

### 使用方式

Agent 在特定场景下使用预定义的 Prompt 模板，确保输出格式和内容符合预期。Prompt 模板由 MCP Server 提供，Agent 不需要自己构造。

## 7. Tool Calling 工作流程

完整的 Tool Calling 流程：

```text
Agent
   ↓
发现 Tool（获取工具列表）
   ↓
选择 Tool（根据任务需求）
   ↓
传递参数（构造调用参数）
   ↓
执行（Host → Client → Server → 外部系统）
   ↓
返回结果（Server → Client → Host → Agent）
   ↓
Agent 继续工作（根据结果决定下一步）
```

### 关键环节说明

1. **发现**：Agent 启动时或任务执行中，获取所有已连接 MCP Server 的工具列表。
2. **选择**：Agent 根据任务需求和工具描述，选择最合适的工具。
3. **参数**：Agent 根据参数定义构造调用参数，确保类型和格式正确。
4. **执行**：Host 将请求路由到对应的 Client，Client 发送给 Server，Server 执行。
5. **返回**：Server 返回执行结果，可能是成功数据或错误信息。
6. **继续**：Agent 根据返回结果决定下一步操作，可能调用更多工具或输出最终结果。

## 8. MCP Server 设计原则

### 单一职责

每个 MCP Server 专注于一类工具。GitHub MCP 只处理 GitHub 相关操作，MySQL MCP 只处理数据库操作。避免把多个不相关的工具塞进同一个 Server。

### 最小暴露

只暴露必要的工具。如果一个外部系统有 100 个 API，但 Agent 只需要其中 5 个，就只暴露这 5 个。

### 清晰描述

每个工具的名称和描述要清晰准确，让 Agent 能正确判断何时使用。模糊的描述会导致 Agent 误用工具。

### 安全边界

明确区分只读工具和写入工具。写入工具需要额外的确认机制和权限控制。

### 错误处理

工具执行失败时返回清晰的错误信息，帮助 Agent 理解问题并决定下一步。

## 9. Tool 设计原则

### 参数最小化

只暴露必要的参数。参数越多，Agent 构造错误参数的风险越高。

### 幂等性

只读工具天然幂等。写入工具尽量设计为幂等操作，避免重复调用产生副作用。

### 超时控制

设置合理的执行超时时间，避免长时间阻塞 Agent 工作流。

### 返回值规范

返回结构化的结果，便于 Agent 解析和使用。避免返回冗长的原始数据。

## 10. 本项目 MCP Server 分类

结合 `mcp/` 目录，本项目的 MCP Server 按功能分为六大类：

### 开发工具

| Server | 用途 | 典型场景 |
| --- | --- | --- |
| adb | Android 设备调试 | 安装 APK、查看日志、分析设备状态 |
| git | Git 版本控制 | 查看提交历史、管理分支、比较差异 |
| gradle | Android 项目构建 | 执行构建、清理、依赖同步 |

### 代码平台

| Server | 用途 | 典型场景 |
| --- | --- | --- |
| github | GitHub 代码托管 | 管理仓库、Issues、Pull Requests、代码审查 |

### 项目管理

| Server | 用途 | 典型场景 |
| --- | --- | --- |
| jira | Jira 项目管理 | 管理需求、任务、Bug、Sprint |

### 文档

| Server | 用途 | 典型场景 |
| --- | --- | --- |
| confluence | Confluence 企业知识库 | 读写技术方案、团队文档 |
| showdoc | ShowDoc 文档平台 | 管理 API 文档、项目文档 |

### 设计

| Server | 用途 | 典型场景 |
| --- | --- | --- |
| figma | Figma 设计工具 | 读取设计稿、获取样式信息、理解页面结构 |

### 数据库

| Server | 用途 | 典型场景 |
| --- | --- | --- |
| mysql | MySQL 数据库 | 查询数据、查看表结构、分析数据 |
| sqlite | SQLite 数据库 | 本地轻量数据库操作、数据分析 |

### 覆盖范围

这 10 个 MCP Server 覆盖了企业研发流程中的核心工具链：

- **代码管理**：Git、GitHub
- **项目管理**：Jira
- **设计协作**：Figma
- **知识管理**：Confluence、ShowDoc
- **数据管理**：MySQL、SQLite
- **构建与调试**：Gradle、ADB

## 本章总结

- MCP Server 是封装外部系统访问逻辑的服务端程序，通过 MCP 协议暴露 Tools。
- MCP Client 是 Host 内部的协议客户端，负责与 Server 通信。
- MCP Host 是发起 MCP 连接的应用程序（AI Agent）。
- Tool 是可执行的能力单元，Resource 是只读数据源，Prompt 是结构化模板。
- Tool Calling 流程：发现 → 选择 → 参数 → 执行 → 返回 → 继续。
- Server 和 Tool 设计遵循单一职责、最小暴露、清晰描述、安全边界原则。
- 本项目 10 个 MCP Server 覆盖开发、代码、项目管理、文档、设计、数据库六大类。

## 参考资料

- MCP 概述：`01-overview.md`
- 安装与环境准备：`02-install.md`
- MCP Workflow：`04-workflow.md`
- MCP 配置：`mcp/` 目录
