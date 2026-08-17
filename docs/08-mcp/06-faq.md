# MCP FAQ

> MCP 使用中的常见问题整理。按概述、架构、Server、Tool、安装、配置、安全、各工具 MCP、调试、故障排查分类。

---

## 概述问题

## Q1: MCP 是什么？

### A

MCP（Model Context Protocol）是一个开放标准协议，用于连接 AI 模型与外部工具、数据源和系统。它让 AI Agent 以标准化方式访问企业内外部能力，而不需要为每个工具编写定制集成代码。

## Q2: 为什么需要 MCP？

### A

AI Agent 的能力需要延伸到本地文件之外。MCP 解决了三个核心问题：工具集成碎片化（统一协议）、能力边界受限（触达外部系统）、企业工具链协同（连接多个平台）。

## Q3: MCP 是一个产品吗？

### A

不是。MCP 是一个开放协议标准，不是一个具体的产品。任何兼容 MCP 协议的 Host 和 Server 都可以互相连接。类似于 HTTP 是 Web 的协议标准，MCP 是 AI 与外部工具连接的协议标准。

## 架构问题

## Q4: MCP Host 是什么？

### A

MCP Host 是发起 MCP 连接的应用程序。在 AI FullStack Workflow 中，AI Coding Agent（如 OpenCode、Codex、Claude Code）就是 MCP Host。Host 负责管理 Client 与 Server 之间的连接生命周期。

## Q5: MCP Client 是什么？

### A

MCP Client 是 Host 内部的协议客户端组件，负责与 MCP Server 建立连接、发送请求、接收响应。每个 Client 对应一个 MCP Server 连接。一个 Host 可以有多个 Client。

## Q6: MCP Server 是什么？

### A

MCP Server 是一个独立运行的服务端程序，封装了对外部系统的访问逻辑，通过 MCP 协议暴露 Tools、Resources 和 Prompts。例如 GitHub MCP Server 封装了 GitHub API，让 Agent 能操作仓库。

## Q7: Tool、Resource、Prompt 有什么区别？

### A

- **Tool**：可执行的能力单元，Agent 可以调用它来执行操作（如创建 Issue、查询数据）。
- **Resource**：只读数据源，Agent 可以读取但不修改（如项目配置、数据库 schema）。
- **Prompt**：预定义的提示词模板，为特定场景提供结构化输入。

## 与传统 API 的区别

## Q8: MCP 和传统 API 有什么区别？

### A

传统 API 是应用之间的通信接口，由开发者编写调用代码。MCP 是 AI Agent 与外部系统的连接协议，由 Agent 自动发现和调用。MCP 在传统 API 之上增加了工具发现、连接管理、上下文传递、安全控制等企业级能力。

## Q9: MCP 和 Function Calling 有什么区别？

### A

Function Calling 是模型层面的能力，让模型输出结构化的函数调用请求。MCP 是协议层面的标准，定义了工具如何被发现、连接、调用和返回结果。Function Calling 是 MCP 的基础能力之一，MCP 在其之上增加了工具发现、连接管理、上下文传递等能力。

## Q10: MCP 和 Plugin 有什么区别？

### A

Plugin 是特定平台的扩展机制，通常与宿主应用强绑定（如 VS Code Plugin 只能在 VS Code 用）。MCP 是跨平台的开放协议，一个 MCP Server 可以同时被多个不同的 Host 使用。

## Server 问题

## Q11: MCP Server 有哪两种运行模式？

### A

- **stdio 模式**：Server 作为 Host 的子进程运行，通过标准输入输出通信。适合本地 MCP Server。
- **HTTP/SSE 模式**：Server 作为独立服务运行，通过网络通信。适合远程 MCP Server。

## Q12: 一个 Host 可以连接多个 MCP Server 吗？

### A

可以。一个 Host 通常会连接多个 MCP Server 以覆盖不同的工具需求。例如 OpenCode 可以同时连接 GitHub、Jira、MySQL 等多个 MCP Server，Agent 在执行任务时按需调用不同 Server 的工具。

## Tool 问题

## Q13: Agent 如何发现可用的 Tool？

### A

Agent 启动时或任务执行中，Host 会获取所有已连接 MCP Server 的工具列表。每个工具包含名称、描述、参数定义等信息，Agent 据此判断何时使用哪个工具。

## Q14: Tool Calling 的完整流程是什么？

### A

1. Agent 发现可用工具列表。
2. Agent 根据任务需求选择合适的工具。
3. Agent 构造工具调用参数。
4. Host 将请求路由到对应的 Client → Server。
5. Server 执行工具并返回结果。
6. Agent 根据结果继续工作。

## 本地与远程

## Q15: 本地 MCP 和远程 MCP 如何选择？

### A

- **本地 MCP**：延迟低、不依赖网络、配置简单。适合开发调试、访问本地资源（Git、SQLite、ADB）。
- **远程 MCP**：团队共享、集中管理凭证、适合企业部署。适合访问企业内部服务（Jira、Confluence）。

## Provider 问题

## Q16: 不同的 AI Agent 都支持 MCP 吗？

### A

主流的 AI Coding Agent 都支持 MCP，包括 OpenCode、Codex、Claude Code、CodeBuddy、Pi Agent。不同 Host 的 MCP 配置方式略有不同，但都遵循相同的 MCP 协议。

## Q17: 同一个 MCP Server 可以被多个 Agent 使用吗？

### A

可以。MCP 是开放协议，同一个 GitHub MCP Server 可以同时被 OpenCode、Codex、Claude Code 使用。这是 MCP 相比 Plugin 的核心优势之一。

## Agent 问题

## Q18: MCP 和 Agent 是什么关系？

### A

MCP 是 Agent 能力的延伸层。Agent 负责理解需求、规划任务、生成代码；MCP 让 Agent 能触达代码之外的真实工具与数据。没有 MCP，Agent 只能操作本地文件；有了 MCP，Agent 可以访问 GitHub、查询数据库、读取设计稿。

## Q19: MCP 会替代 Agent 吗？

### A

不会。MCP 和 Agent 是互补关系，不是竞争关系。MCP 是连接协议，Agent 是执行智能体。MCP 扩展了 Agent 的能力边界，但 Agent 的核心（理解、规划、执行）不依赖 MCP。

## OpenCode 相关

## Q20: OpenCode 如何配置 MCP？

### A

在 OpenCode 的配置文件中声明 MCP Server。配置包含 Server 名称、启动命令、环境变量等。OpenCode 启动时会自动连接配置的 MCP Server，并将可用工具暴露给 Agent。

## Q21: OpenCode 可以同时使用多个 MCP Server 吗？

### A

可以。OpenCode 支持在配置文件中声明多个 MCP Server，内部为每个 Server 创建独立的 Client 进行连接管理。

## Codex / Claude Code 相关

## Q22: Codex 的 MCP 配置和 OpenCode 一样吗？

### A

核心概念一致（都是声明 MCP Server），但具体配置格式和位置可能不同。每个 Host 有自己的配置方式，以各自官方文档为准。

## Q23: Claude Code 支持哪些 MCP Server？

### A

Claude Code 支持所有兼容 MCP 协议的 Server。在本项目中，包括 ADB、Git、GitHub、Gradle、Jira、Figma、Confluence、ShowDoc、MySQL、SQLite。

## 安全问题

## Q24: MCP 的写操作安全吗？

### A

需要谨慎管理。写操作（创建 Issue、修改数据库、更新文档）存在误操作风险。建议：写操作前人工确认、设置权限边界、记录审计日志、使用最小权限原则。

## Q25: MCP 的凭证如何安全管理？

### A

所有 API Key、Token 通过环境变量传递，禁止写入配置文件或提交到代码仓库。企业环境使用密钥管理系统统一托管，定期轮换凭证。

## Q26: MCP Server 有数据泄露风险吗？

### A

任何网络连接都有潜在风险。防护措施：远程 MCP 使用 HTTPS 加密传输、敏感数据在上下文中脱敏处理、审计日志不记录敏感原始值、按数据敏感度分级管理权限。

## 权限问题

## Q27: 如何控制 MCP Server 的权限？

### A

通过最小权限原则：每个 MCP Server 只暴露完成其职责所必需的最小工具集。生产环境限制高危操作，写操作需要人工审批。按环境（开发/测试/生产）差异化配置权限。

## Q28: 企业网络环境如何使用 MCP？

### A

企业网络可能限制外部连接。处理方式：配置 HTTP 代理环境变量、确认代理允许 MCP Server 的网络访问、内部 MCP Server 部署在企业内网、必要时联系网络管理员开放白名单。

## 数据库 MCP

## Q29: MySQL MCP 可以执行写操作吗？

### A

技术上可以，但建议生产环境只读。开发环境可以使用写操作进行调试，生产环境应限制为只读查询，写操作通过正式的数据迁移流程执行。

## Q30: SQLite MCP 和 MySQL MCP 有什么区别？

### A

SQLite MCP 操作本地轻量数据库文件，适合开发调试和数据分析。MySQL MCP 连接 MySQL 服务，适合访问团队共享的数据库。两者遵循相同的 MCP 协议，但连接方式和使用场景不同。

## Git / GitHub MCP

## Q31: Git MCP 和 GitHub MCP 有什么区别？

### A

Git MCP 操作本地 Git 仓库（提交历史、分支管理、差异比较）。GitHub MCP 操作 GitHub 平台（仓库管理、Issue、PR、代码审查）。两者互补：Git 管理本地版本控制，GitHub 管理远程协作。

## Q32: GitHub MCP 可以自动合并 PR 吗？

### A

技术上可以，但不建议自动合并。PR 合并应该经过代码审查后由人决定。Agent 可以创建 PR、关联 Issue、添加标签，但合并决策留给人。

## Figma MCP

## Q33: Figma MCP 能做什么？

### A

Figma MCP 让 Agent 读取设计稿信息，包括页面布局结构、颜色/字体/间距等样式信息、组件层级关系。Agent 可以据此生成更贴合设计意图的 UI 代码。

## Jira MCP

## Q34: Jira MCP 可以自动创建任务吗？

### A

可以。Agent 通过 Jira MCP 可以读取需求、创建任务、更新状态、记录工作日志。但任务的优先级排序和资源分配应该由人决定。

## Confluence / ShowDoc MCP

## Q35: Confluence MCP 和 ShowDoc MCP 有什么区别？

### A

Confluence MCP 连接 Confluence 企业知识库，适合团队内部技术方案、知识沉淀。ShowDoc MCP 连接 ShowDoc 文档平台，适合 API 文档、项目文档管理。两者都支持文档的读写操作。

## ADB / Gradle MCP

## Q36: ADB MCP 可以做哪些事？

### A

ADB MCP 封装了 Android 调试桥的能力：安装 APK、查看设备日志、分析设备状态、执行 shell 命令。让 Agent 能参与 Android 应用的调试环节。

## Q37: Gradle MCP 可以做什么？

### A

Gradle MCP 封装了 Gradle 构建工具的能力：执行项目构建、清理、依赖同步。让 Agent 能自动触发 Android 项目的构建流程。

## 调试问题

## Q38: MCP Server 连接失败怎么办？

### A

按顺序排查：
1. 检查 MCP Server 启动命令是否正确。
2. 检查环境变量是否已导出。
3. 检查网络连接（远程 Server）。
4. 查看 MCP Server 启动日志。
5. 使用 MCP Inspector 手动测试。

## Q39: MCP 工具调用返回错误怎么办？

### A

1. 查看错误信息，定位失败原因（参数错误、权限不足、网络问题）。
2. 检查工具参数是否符合定义。
3. 检查凭证是否有效、权限是否足够。
4. 查看 MCP Server 日志获取更多细节。

## Q40: 如何调试 MCP Server？

### A

- 单独启动 MCP Server，查看终端输出。
- 使用 MCP Inspector 工具手动测试工具调用。
- 查看 Host 的 MCP 连接日志。
- 检查环境变量和网络连接。

## 故障排查

## Q41: MCP Server 启动后工具列表为空怎么办？

### A

- 检查 MCP Server 是否正常启动。
- 查看 Server 日志是否有注册工具的输出。
- 确认 MCP 协议版本兼容。
- 检查 Server 配置是否正确。

## Q42: MCP 工具调用超时怎么办？

### A

- 检查网络连接（远程 Server）。
- 确认 MCP Server 是否正常运行。
- 检查工具执行是否有长时间阻塞。
- 增加超时时间或优化工具执行效率。

## Q43: 多个 MCP Server 冲突怎么办？

### A

- 确认每个 Server 的名称唯一。
- 检查是否有端口冲突（远程 Server）。
- 分别测试每个 Server，排除单个 Server 的问题。

## 总结

- MCP 是连接协议，不是产品，与 API、Plugin、Function Calling 有本质区别。
- Host/Client/Server 三层架构，Tool/Resource/Prompt 三种能力。
- 本地 MCP 适合开发调试，远程 MCP 适合团队共享。
- 安全是核心：凭证走环境变量、最小权限、写操作确认、审计日志。
- 调试从启动日志、Inspector、环境变量三处入手。
- 每个工具 MCP 各有侧重，按团队工具链选择接入。

## 参考资料

- MCP 概述：`01-overview.md`
- 安装与环境准备：`02-install.md`
- MCP Server 与 Tools：`03-server-and-tools.md`
- 最佳实践：`05-best-practice.md`
