# Project Architecture

> 本文档说明整个项目的目录结构、每个一级目录的职责与维护原则。

---

## 目录总览

```
AI-FullStack-Workflow/
├── README.md            # 项目首页
├── CHANGELOG.md         # 变更记录
├── CONTRIBUTING.md      # 贡献指南
├── LICENSE              # MIT 许可证
├── .github/             # GitHub 社区标准（模板 / CI）
├── docs/                # 正式文档
├── prompts/             # Prompt 库
├── rules/               # Rules 库
├── agents/              # Agent 库
├── mcp/                 # MCP 配置
├── examples/            # 案例
├── templates/           # 模板
├── scripts/             # 脚本
└── assets/              # 资源
```

---

## docs — 正式文档

### 作用

存放全部章节知识文档，是项目的核心内容资产。

### 为什么存在

知识库的主体现就是文档。`docs/` 按主题分章节（`00-roadmap` 到 `16-case-study`），让读者可以按路线图循序渐进地阅读。

### 以后放什么

- `00-roadmap/`：项目定位、架构、路线图、写作规范。
- `01-ai-basic/` 到 `02-models/`：AI 基础与大模型。
- `03-opencode/` 到 `06-codebuddy/`：各 AI Coding 工具。
- `08-mcp/` 到 `11-agent/`：MCP、规则、Prompt、Agent 原理。
- `12-flutter/` 到 `14-vue/`：全栈实战。
- `15-enterprise/`、`16-case-study/`：企业研发与案例。

### 维护原则

- 每章以 `index.md` 为主文档，按需配套 `architecture.md`、`prompts.md`、`checklist.md`、`faq.md`、`examples.md`。
- 所有文档遵循 `writing-style.md` 的写作规范。

---

## prompts — Prompt 库

### 作用

存放可直接复用的 Prompt 模板，按技术栈与场景分类。

### 为什么存在

Prompt 是 AI 开发中最重要也最容易被随意对待的资产。集中管理、统一规范，才能保证产出质量稳定、可复用。

### 以后放什么

- 按技术栈：`flutter/`、`android/`、`kotlin/`、`java/`、`dart/`、`vue/`。
- 按场景：`review/`、`refactor/`、`architecture/`、`product/`、`ui/`、`test/`。

### 维护原则

- 每个 Prompt 文件遵循 `templates/prompt-template.md` 的结构。
- 新 Prompt 必须能明确说明适用场景与使用方式。

---

## rules — Rules 库

### 作用

存放编码规范、审查规范、提交规范等约束规则，是 AI 与人类开发者的共同行为准则。

### 为什么存在

Rules 是让 AI 产出"符合团队预期"的关键。没有规则约束，AI 生成的代码风格千奇百怪；有了规则，团队才能统一标准、降低审查成本。

### 以后放什么

- `flutter/`、`android/`、`kotlin/`、`java/`、`dart/`、`vue/`：各技术栈编码规范。
- `git/`：Git 提交与分支规范。
- `review/`：代码审查规范。

### 维护原则

- 每条规则必须可执行、可检查，避免模糊表述。
- 规则变更需同步更新相关 Prompt 与 Agent 引用。

---

## agents — Agent 库

### 作用

存放各技术领域 Agent 的配置，包括系统提示词（`prompt.md`）与工作流程（`workflow.md`）。

### 为什么存在

Agent 是把"能力"固化为可复用资产的方式。一次配置、长期复用，团队每个成员都能获得一致的 AI 协作体验。

### 以后放什么

- `flutter-agent/`、`android-agent/`、`architect-agent/` 等技术 Agent。
- `review-agent/`、`product-agent/`、`release-agent/`、`test-agent/`、`git-agent/` 等流程 Agent。

### 维护原则

- 每个 Agent 一个目录，至少包含 `README.md`、`prompt.md`、`workflow.md`。
- Agent 与对应技术栈的 `rules/`、`prompts/` 保持同步。

---

## mcp — MCP 配置

### 作用

存放 MCP（Model Context Protocol）服务器配置与使用说明。

### 为什么存在

MCP 让 AI 连接外部工具与数据（GitHub、数据库、设计稿等），是打通"AI 与真实世界"的关键环节。

### 以后放什么

- `github/`、`git/`、`figma/`、`jira/`、`adb/`、`gradle/` 等工具 MCP。
- `sqlite/`、`mysql/` 等数据库 MCP。
- `showdoc/`、`confluence/` 等文档 MCP。

### 维护原则

- 每个 MCP 一个目录，包含安装、配置、用法说明。
- 配置中的密钥信息一律引用环境变量，禁止写入仓库。

---

## examples — 案例

### 作用

存放可运行的完整示例项目，作为文档中"实战案例"部分的落地参照。

### 为什么存在

光有理论不落地是教程的老毛病。可运行示例让读者（和 AI）能直接对照验证。

### 以后放什么

- 各章节配套的迷你项目。
- 演示特定 Prompt / Agent / 工作流的端到端示例。

### 维护原则

- 每个示例必须可运行，并在 `README.md` 中说明运行方式与依赖。

---

## templates — 模板

### 作用

存放各类标准模板（章节、Prompt、Rule、Agent、Workflow、案例）。

### 为什么存在

模板是"知识库自举"的引擎。有了模板，新增内容才能保持统一结构与质量，越写越规范。

### 以后放什么

- `chapter-template.md`、`prompt-template.md`、`rule-template.md`。
- `agent-template.md`、`workflow-template.md`、`case-template.md`。

### 维护原则

- 模板与 `writing-style.md` 保持一致，模板变更需同步规范文档。

---

## scripts — 脚本

### 作用

存放自动化脚本（目录检查、文档统计、CI 辅助等）。

### 为什么存在

把重复性工作交给脚本，减少人工操作，保证仓库一致性。

### 以后放什么

- 目录结构检查脚本。
- Markdown 规范检查脚本。
- 文档统计与索引生成脚本。

### 维护原则

- 脚本必须可在本机与 CI 中运行，并附使用说明。

---

## assets — 资源

### 作用

存放静态资源，主要是图片（`assets/images/`）。

### 为什么存在

写作规范要求"禁止网络图片"，所有图片必须入库自托管，保证文档长期可访问。

### 以后放什么

- `assets/images/`：所有文档引用的图片。
- 命名规范：`章节-序号-描述`，如 `opencode-01-install.png`。

### 维护原则

- 图片命名遵循规范，引用路径与文档目录结构对应。

---

## Git Commit 规范

统一格式：

```text
类型: 中文描述
```

Commit 类型只使用以下几种：

| 类型 | 用途 | 示例 |
| --- | --- | --- |
| `docs` | 文档 | docs: 新增 AI 开发时代章节 |
| `feat` | 新功能/新增内容 | feat: 新增 Flutter Agent |
| `fix` | 修复 | fix: 修复 README 链接错误 |
| `refactor` | 重构 | refactor: 优化 Prompt 目录结构 |
| `style` | 格式调整 | style: 统一 Markdown 标题格式 |
| `chore` | 杂项 | chore: 更新项目配置 |

规范要点：

1. **采用 Conventional Commit + 中文描述**，既保留行业通用类型，又兼顾中文维护可读性。
2. **Commit 不要太大**，一次只做一件事：
   - ❌ 不建议：`docs: 更新所有文档`
   - ✅ 建议：`docs: 新增 AI 开发时代章节`、`fix: 修复 MkDocs 导航错误`
3. 清晰的小提交让 Git 历史可追溯，方便定位变更。
