# MCP 在 AI FullStack Workflow 中的应用

> 本章节核心。围绕真实研发流程，讲解 MCP 如何改变 AI Agent 的能力边界，让 Agent 从"只能操作本地文件"升级为"能触达企业全工具链"。

---

## 为什么需要了解

MCP 的价值不在协议本身，而在于它如何融入真实开发流程。本文给出不同场景下的 MCP 工作流，让团队理解 MCP 如何在 Android 开发、Flutter 开发、UI 开发、文档管理、Git 操作、项目管理、数据库操作中发挥作用，并最终形成完整的企业研发工作流。

## 1. 基础 Workflow

MCP 驱动的基础工作流：

```text
AI（理解需求）
   ↓
Agent（规划任务）
   ↓
MCP（发现工具、调用工具）
   ↓
Tool（执行操作）
   ↓
Result（返回结果）
   ↓
Agent（继续工作 / 验证 / 反馈）
```

### 核心循环

1. **AI 理解需求**：模型分析任务，理解目标。
2. **Agent 规划**：Agent 决定需要哪些外部工具，选择对应的 MCP Server。
3. **MCP 调用**：Agent 通过 MCP 协议调用工具，获取外部数据或执行操作。
4. **Tool 执行**：MCP Server 执行工具，访问外部系统。
5. **Result 返回**：执行结果返回给 Agent。
6. **Agent 继续**：Agent 根据结果决定下一步，可能再次调用工具或输出最终成果。

### 与无 MCP 的对比

| 维度 | 无 MCP | 有 MCP |
| --- | --- | --- |
| 工具访问 | 只能操作本地文件 | 访问企业全工具链 |
| 数据来源 | 靠人提供上下文 | 自动获取外部数据 |
| 操作范围 | 读写代码文件 | 代码 + 项目管理 + 设计 + 数据库 |
| 工作流 | 编码单环节 | 覆盖研发全流程 |

## 2. Android 开发 Workflow

结合 ADB、Gradle、Git 三个 MCP Server，AI Agent 能覆盖 Android 开发的编码、构建、安装、调试全流程。

### 工作流

```text
代码修改（Agent 生成 / 修改代码）
   ↓
Gradle（构建项目、生成 APK）
   ↓
ADB（安装 APK 到设备）
   ↓
ADB（运行验证、查看日志）
   ↓
日志分析（Agent 分析日志、定位问题）
   ↓
迭代修复（Agent 根据日志继续修改）
```

### 场景示例

**场景：修复一个 Android 崩溃**

1. Agent 读取崩溃日志（ADB MCP 获取设备日志）。
2. Agent 分析日志，定位崩溃原因。
3. Agent 修改代码（本地文件操作）。
4. Agent 触发 Gradle 构建（Gradle MCP）。
5. Agent 通过 ADB 安装新 APK（ADB MCP）。
6. Agent 再次获取日志验证修复（ADB MCP）。
7. 验证通过，Agent 提交代码（Git MCP）。

### 覆盖的 MCP Server

- **ADB**：设备连接、APK 安装、日志查看、设备状态分析
- **Gradle**：项目构建、依赖管理、清理
- **Git**：代码提交、分支管理、版本控制

## 3. Flutter 开发 Workflow

Flutter 开发的特点是跨平台构建、热重载、状态管理复杂。

### 工作流

```text
代码修改（Agent 生成 / 修改 Dart 代码）
   ↓
Flutter Build（构建 APK / IPA）
   ↓
设备部署（ADB / 模拟器）
   ↓
运行验证（Agent 观察运行状态）
   ↓
日志分析（Agent 分析 Flutter 日志）
   ↓
迭代修复
```

### 场景示例

**场景：开发一个新的 Flutter 页面**

1. Agent 读取设计稿（Figma MCP），理解页面结构。
2. Agent 生成页面代码（Controller / View / Binding）。
3. Agent 触发 Flutter 构建。
4. Agent 通过 ADB 安装到测试设备。
5. Agent 获取运行日志，分析是否有异常。
6. 验证通过后提交代码（Git MCP）。

### 关键要点

- Figma MCP 让 Agent 能读取设计意图，生成更贴合设计的 UI 代码。
- ADB MCP 让 Agent 能在真机上验证 Flutter 应用。
- Git MCP 让 Agent 能完成从编码到提交的完整闭环。

## 4. UI 开发 Workflow

结合 Figma MCP，AI Agent 能从设计稿直接理解页面结构和样式信息。

### 工作流

```text
Figma（读取设计稿、提取样式信息）
   ↓
AI（理解设计意图、页面结构）
   ↓
Agent（生成 UI 代码）
   ↓
Review（对照设计稿审查代码）
   ↓
Git（提交代码）
```

### 场景示例

**场景：根据设计稿实现新页面**

1. Agent 通过 Figma MCP 获取设计稿信息：
   - 页面布局结构
   - 颜色、字体、间距等样式信息
   - 组件层级关系
2. Agent 生成对应的 UI 代码，遵循设计规范。
3. 人工或 AI 对照设计稿审查代码实现。
4. 验证通过后提交。

### 关键要点

- Figma MCP 让 Agent "看到"设计稿，而不是靠人描述。
- 减少了"设计 → 开发"之间的信息损耗。
- Agent 生成的代码更贴合设计意图。

## 5. 文档 Workflow

结合 Confluence 和 ShowDoc MCP，AI Agent 能参与文档管理流程。

### 工作流

```text
Confluence / ShowDoc（读取现有文档）
   ↓
Agent（理解文档结构与内容）
   ↓
代码变更（Agent 修改代码）
   ↓
Agent（更新相关文档）
   ↓
Confluence / ShowDoc（写入更新后的文档）
```

### 场景示例

**场景：代码变更后更新 API 文档**

1. Agent 读取现有 API 文档（Confluence MCP 或 ShowDoc MCP）。
2. Agent 读取代码变更，理解新增/修改的接口。
3. Agent 更新 API 文档，补充新接口说明。
4. 更新后的文档写回 Confluence 或 ShowDoc。

### 关键要点

- 文档 MCP 让 Agent 能保持文档与代码同步。
- 减少了"代码改了、文档没更新"的情况。
- Agent 可以自动生成变更日志和发布说明。

## 6. Git Workflow

结合 Git 和 GitHub MCP，AI Agent 能覆盖完整的版本控制流程。

### 工作流

```text
Git（读取仓库状态、分支、提交历史）
   ↓
Agent（理解当前状态、规划改动）
   ↓
Agent（修改代码）
   ↓
Git（提交改动、管理分支）
   ↓
GitHub（创建 PR、关联 Issue）
   ↓
GitHub（代码审查、合并）
```

### 场景示例

**场景：开发一个新功能并提交 PR**

1. Agent 通过 Git MCP 了解当前分支和提交历史。
2. Agent 创建功能分支（Git MCP）。
3. Agent 完成代码开发（本地文件操作）。
4. Agent 提交代码（Git MCP）。
5. Agent 通过 GitHub MCP 创建 Pull Request。
6. Agent 关联相关的 Issue（GitHub MCP）。
7. 团队成员审查代码，Agent 根据审查意见修改。

### 关键要点

- Git MCP 让 Agent 能管理版本控制，不只是修改文件。
- GitHub MCP 让 Agent 能参与代码审查流程。
- 完整的"开发 → 提交 → PR → 审查"闭环。

## 7. 项目管理 Workflow

结合 Jira MCP，AI Agent 能参与项目管理流程。

### 工作流

```text
Jira（读取需求、任务、Bug）
   ↓
Agent（理解任务要求、规划实现）
   ↓
Agent（完成开发）
   ↓
Jira（更新任务状态、记录工作日志）
   ↓
Agent（创建关联的子任务或测试任务）
```

### 场景示例

**场景：处理一个 Jira 任务**

1. Agent 通过 Jira MCP 读取任务详情。
2. Agent 理解任务要求，规划实现步骤。
3. Agent 完成代码开发。
4. Agent 通过 Jira MCP 更新任务状态为"开发完成"。
5. Agent 记录工作日志到 Jira。

### 关键要点

- Jira MCP 让 AI 参与项目管理流程，不只是编码。
- Agent 能自动更新任务状态，减少人工操作。
- 任务与代码变更可以自动关联。

## 8. 数据库 Workflow

结合 MySQL 和 SQLite MCP，AI Agent 能参与数据库相关开发。

### 工作流

```text
MySQL / SQLite（读取表结构、数据）
   ↓
Agent（理解数据模型、编写查询）
   ↓
Agent（生成数据层代码）
   ↓
MySQL / SQLite（验证查询结果）
   ↓
Agent（根据结果调整代码）
```

### 场景示例

**场景：为新功能编写数据层代码**

1. Agent 通过 MySQL MCP 读取数据库表结构。
2. Agent 理解数据模型和字段关系。
3. Agent 生成对应的 Model / Repository 代码。
4. Agent 通过 MySQL MCP 执行查询验证。
5. 验证通过后提交代码。

### 关键要点

- 数据库 MCP 让 Agent 基于真实数据模型生成代码，而不是猜测。
- Agent 能直接验证生成的 SQL 是否正确。
- 减少了"代码写好了、数据库对不上"的情况。

## 9. 企业研发 Workflow

将所有 MCP Server 组合，形成完整的企业研发工作流：

```text
需求（Jira MCP 读取需求）
   ↓
设计（Figma MCP 读取设计稿）
   ↓
开发（Agent 生成代码 + Git MCP 管理版本）
   ↓
构建（Gradle MCP 构建项目）
   ↓
测试（ADB MCP 安装验证 / 数据库 MCP 查询验证）
   ↓
文档（Confluence / ShowDoc MCP 更新文档）
   ↓
发布（GitHub MCP 创建 PR、合并）
   ↓
项目管理（Jira MCP 更新任务状态）
```

### 完整闭环

1. **需求阶段**：Agent 通过 Jira MCP 读取需求详情，理解任务目标。
2. **设计阶段**：Agent 通过 Figma MCP 读取设计稿，理解页面结构。
3. **开发阶段**：Agent 生成代码，通过 Git MCP 管理版本。
4. **构建阶段**：Agent 通过 Gradle MCP 触发构建。
5. **测试阶段**：Agent 通过 ADB MCP 安装到设备验证，通过数据库 MCP 查询验证数据。
6. **文档阶段**：Agent 通过 Confluence / ShowDoc MCP 更新相关文档。
7. **发布阶段**：Agent 通过 GitHub MCP 创建 PR，团队审查后合并。
8. **管理阶段**：Agent 通过 Jira MCP 更新任务状态，记录完成情况。

### 关键价值

- Agent 不再只是"写代码的工具"，而是参与研发全流程的"智能协作者"。
- 工具链之间的信息自动流转，减少人工搬运。
- 每个环节都有对应的 MCP Server 支撑，形成完整的工具链闭环。

## 10. 完整 AI FullStack Workflow

最终形成的完整工作流：

```text
Model（理解与推理）
   ↓
Agent（规划与执行）
   ↓
MCP（工具发现与调用）
   ↓
企业工具（GitHub / Jira / Figma / 数据库 / ...）
   ↓
真实研发流程（需求 → 设计 → 开发 → 测试 → 文档 → 发布）
   ↓
验证与反馈
   ↓
Agent 继续工作
```

### 三层架构

1. **模型层**：提供理解与推理能力（参考 `docs/02-models/`）。
2. **Agent 层**：提供规划与执行能力（参考 `docs/03-opencode/` 等章节）。
3. **MCP 层**：提供工具连接能力（本章节）。

### 核心理念

- **AI 不替代人，而是增强人**：Agent 执行重复性工作，人把控关键决策。
- **工具链不孤岛，而是协同**：MCP 让所有工具通过统一协议连接。
- **工作流不手动，而是自动化**：从需求到发布，Agent 参与每个环节。

## 本章总结

- MCP 让 Agent 从"操作本地文件"升级为"触达企业全工具链"。
- Android 开发：ADB + Gradle + Git 覆盖编码、构建、安装、调试。
- Flutter 开发：Figma + ADB + Git 覆盖设计理解、编码、验证。
- UI 开发：Figma MCP 让 Agent "看到"设计稿，减少信息损耗。
- 文档管理：Confluence + ShowDoc MCP 保持文档与代码同步。
- Git 操作：Git + GitHub MCP 覆盖版本控制与代码审查。
- 项目管理：Jira MCP 让 Agent 参与需求管理流程。
- 数据库：MySQL + SQLite MCP 让 Agent 基于真实数据模型生成代码。
- 企业研发：所有 MCP Server 组合，形成完整闭环。

## 参考资料

- MCP 概述：`01-overview.md`
- MCP Server 与 Tools：`03-server-and-tools.md`
- 最佳实践：`05-best-practice.md`
- MCP 配置：`mcp/` 目录
