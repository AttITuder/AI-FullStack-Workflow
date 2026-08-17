# MCP 安装与环境准备

> 面向实际开发者的 MCP 环境搭建指南。重点是建立 MCP 的基础运行环境，不是介绍某一个 MCP Server 的具体配置。

---

## 为什么需要了解

MCP 是连接 AI Agent 与外部工具的基础设施。只有先搭建好 MCP 运行环境，Agent 才能发现和调用外部工具。本文覆盖 MCP 运行环境要求、Host/Client/Server 关系、安装方式、配置方法与常见问题。

## 1. MCP 使用环境

### 操作系统

- 推荐：macOS、Linux（WSL/Linux 桌面）。
- 支持：Windows（建议使用 WSL 以获得完整体验）。
- MCP Server 本身是独立进程，多数基于 Node.js 或 Python 实现，跨平台兼容性好。

### 运行时环境

MCP Server 基于不同技术栈实现，常见运行时：

- **Node.js**：大部分 MCP Server 基于 TypeScript/JavaScript 实现。
- **Python**：部分 MCP Server 基于 Python 实现。
- 建议通过版本管理工具（如 nvm、pyenv）管理运行时版本。

### MCP Host

MCP Host 是发起 MCP 连接的应用程序。在本项目中，AI Coding Agent 就是 MCP Host：

- OpenCode
- Codex
- Claude Code
- CodeBuddy
- Pi Agent

每个 Host 的 MCP 配置方式略有不同，但都遵循相同的 MCP 协议。

### 网络环境

- 本地 MCP Server 通常不需要外网访问。
- 远程 MCP Server 需要稳定的网络连接。
- 企业网络可能需要代理配置。

## 2. Host / Client / Server 的关系

MCP 的运行架构由三层组成：

```text
MCP Host（AI Agent 应用）
   ↓ 管理
MCP Client（协议客户端）
   ↓ 连接
MCP Server（工具服务端）
   ↓ 封装
外部工具 / 数据源
```

### 举例说明

以 OpenCode + GitHub MCP Server 为例：

1. **Host**：OpenCode 启动后，加载 MCP 配置，发现需要连接 GitHub MCP Server。
2. **Client**：OpenCode 内部创建一个 MCP Client，按照 MCP 协议与 GitHub MCP Server 建立连接。
3. **Server**：GitHub MCP Server 启动后，暴露 `create_issue`、`list_pr`、`read_file` 等工具。
4. **调用**：当 Agent 需要操作 GitHub 时，Client 向 Server 发送工具调用请求，Server 执行后返回结果。

### 一个 Host 可以连接多个 Server

OpenCode 可以同时连接 GitHub、Jira、MySQL 等多个 MCP Server，Agent 在执行任务时按需调用不同 Server 的工具。

## 3. MCP Server 的安装方式

### 包管理器安装

多数 MCP Server 通过包管理器安装：

```bash
# Node.js 实现的 MCP Server
npm install -g <mcp-server-package>

# 或通过 npx 直接运行
npx <mcp-server-package>
```

### 源码安装

部分 MCP Server 需要从源码安装：

```bash
git clone <mcp-server-repo>
cd <mcp-server-repo>
npm install
# 或
pip install -e .
```

### 容器化安装

企业环境中，MCP Server 可以通过 Docker 容器化部署，确保环境一致性。

### 验证安装

安装完成后，可以通过 MCP Inspector 或直接启动 Server 来验证安装是否成功。

## 4. 本地 MCP Server

本地 MCP Server 运行在开发者机器上，适合：

- 访问本地文件系统（如 Git 仓库）。
- 操作本地数据库（如 SQLite）。
- 连接本地设备（如 ADB 连接的 Android 设备）。
- 开发调试阶段。

### 启动方式

本地 MCP Server 通常通过 stdio（标准输入输出）与 Host 通信，由 Host 自动启动和管理。

### 优点

- 延迟低、响应快。
- 不依赖网络。
- 配置简单。

### 注意事项

- 需要在每台开发机器上安装。
- 不适合团队共享同一个 Server 实例。

## 5. 远程 MCP Server

远程 MCP Server 运行在独立服务器上，通过网络（HTTP/SSE）与 Host 通信，适合：

- 团队共享同一个 Server 实例。
- 需要访问企业内部服务（如 Jira、Confluence）。
- 生产环境部署。

### 通信方式

- **stdio**：本地进程通信，由 Host 启动 Server 子进程。
- **HTTP/SSE**：网络通信，Server 作为独立服务运行。

### 优点

- 团队共享，配置一致。
- 集中管理凭证和权限。
- 适合企业级部署。

### 注意事项

- 需要稳定的网络连接。
- 需要考虑安全认证和传输加密。
- 延迟高于本地通信。

## 6. 配置 MCP

### 配置文件位置

MCP 配置通常在 Host 的配置文件中声明。不同 Host 的配置位置和格式略有差异，但核心信息一致：

- Server 名称
- 启动命令或 URL
- 环境变量（凭证等）
- 工作目录（如需要）

### 配置结构

一个典型的 MCP 配置包含：

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["<mcp-server-package>"],
      "env": {
        "API_KEY": "${API_KEY_ENV_VAR}"
      }
    }
  }
}
```

### 配置原则

- 凭证通过环境变量引用，不写入配置文件。
- 每个 Server 配置明确用途与权限。
- 团队共享的配置纳入版本库，个人敏感配置用 `.env` 管理。

## 7. 环境变量与凭证

### 凭证管理原则

- 所有 API Key、Token 通过环境变量传递。
- 禁止将真实凭证写入配置文件或提交到代码仓库。
- 使用 `.env` 文件管理本地开发凭证，确保 `.env` 在 `.gitignore` 中。

### 环境变量设置

```bash
# .env 文件
GITHUB_TOKEN=ghp_xxxxxxxxxxxx
JIRA_API_TOKEN=xxxxxxxx
MYSQL_PASSWORD=xxxxxxxx
```

### 团队凭证管理

- 企业环境建议使用密钥管理系统（如 Vault、AWS Secrets Manager）。
- 团队成员通过个人凭证访问，不共享同一套密钥。
- 定期轮换凭证，过期凭证及时吊销。

## 8. 项目中如何组织 MCP

本项目在 `mcp/` 目录中按工具类型组织 MCP Server 配置：

```text
mcp/
  ├── adb/          # Android 调试桥
  ├── confluence/   # Confluence 文档
  ├── figma/        # Figma 设计稿
  ├── git/          # Git 版本控制
  ├── github/       # GitHub 代码平台
  ├── gradle/       # Gradle 构建
  ├── jira/         # Jira 项目管理
  ├── mysql/        # MySQL 数据库
  ├── showdoc/      # ShowDoc 文档
  ├── sqlite/       # SQLite 数据库
  └── README.md     # MCP 目录说明
```

每个子目录对应一个 MCP Server 的配置与说明。团队成员根据自己的工具链选择需要的 MCP Server 进行配置。

### 接入步骤

1. 确定需要哪些 MCP Server（按团队工具链选择）。
2. 按 `mcp/<server>/` 目录中的说明完成安装与配置。
3. 在 Host 配置中声明 MCP Server。
4. 验证连接是否成功。

## 9. MCP Server 调试

### 启动日志

MCP Server 启动时通常会输出日志，包括：

- 已注册的工具列表
- 连接状态
- 错误信息

### MCP Inspector

MCP 官方提供了 Inspector 工具，用于测试和调试 MCP Server：

- 查看 Server 暴露的工具列表
- 手动调用工具并查看返回结果
- 检查连接状态和配置

### 常用调试方法

1. **单独启动 Server**：在终端直接运行 Server 命令，查看输出是否正常。
2. **检查配置**：确认 Host 配置中的 Server 名称、命令、参数是否正确。
3. **查看环境变量**：确认凭证环境变量已正确导出。
4. **检查网络**：远程 Server 需要确认网络连通性和端口可达。

## 10. 常见安装问题

### Node.js 相关

**问题**：MCP Server 安装时提示 Node 版本不兼容。

**处理**：通过版本管理工具（nvm/fnm）切换到符合要求的 Node 版本。

### Python 相关

**问题**：Python 实现的 MCP Server 安装时依赖冲突。

**处理**：使用虚拟环境隔离依赖，避免与系统 Python 冲突。

### 网络问题

**问题**：远程 MCP Server 连接超时。

**处理**：
- 检查网络连接与代理设置。
- 确认 Server 端口未被防火墙阻断。
- 企业网络可能需要配置代理。

### 权限问题

**问题**：MCP Server 启动时报权限错误。

**处理**：
- 检查文件和目录的访问权限。
- Node.js 全局安装的权限问题参考 OpenCode 安装章节的处理方式。

### 环境变量问题

**问题**：MCP Server 启动后无法访问外部服务。

**处理**：
- 确认环境变量名与 Server 配置中引用的一致。
- 确认环境变量已导出到当前会话。
- 部分 Host 需要重启会话才能加载新的环境变量。

### 企业网络问题

**问题**：企业代理环境下 MCP Server 无法连接外部服务。

**处理**：
- 配置 HTTP 代理环境变量（`HTTP_PROXY` / `HTTPS_PROXY`）。
- 确认代理允许 MCP Server 的网络访问。
- 必要时联系网络管理员开放白名单。

## 本章总结

- MCP 运行环境需要 Node.js 或 Python 运行时，推荐 macOS/Linux。
- Host → Client → Server 三层架构，Host 管理连接，Server 封装工具。
- 安装方式包括包管理器、源码安装、容器化部署。
- 本地 Server 适合开发调试，远程 Server 适合团队共享。
- 凭证一律走环境变量，禁止硬编码。
- 项目 `mcp/` 目录按工具类型组织配置，团队按需选择接入。
- 调试从启动日志、Inspector、环境变量三处入手。

## 参考资料

- MCP 概述：`01-overview.md`
- MCP Server 与 Tools：`03-server-and-tools.md`
- 最佳实践：`05-best-practice.md`
