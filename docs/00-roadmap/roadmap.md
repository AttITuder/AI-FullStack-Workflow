# AI FullStack Workflow Roadmap

> 本路线图是知识库的导航图。以 Milestone 为单位推进，每个 Milestone 包含目标、预计时间、交付内容与完成标准。

---

## Milestone 0：仓库基建（已完成）

### 目标

建立企业级仓库骨架，让后续内容建设有一个稳定、规范的落点。

### 预计时间

Day 1 ~ Day 2。

### 交付内容

- 顶层目录：`docs`、`prompts`、`rules`、`agents`、`mcp`、`examples`、`templates`、`scripts`、`assets`。
- 每个目录的 `README.md` 占位说明。
- GitHub 社区标准：Issue 模板、PR 模板、CI workflow。
- 基础文件：`CHANGELOG.md`、`CONTRIBUTING.md`、`LICENSE`（MIT）、`.gitignore`、`.editorconfig`。

### 完成标准

- 所有目录均有 README，CI 可运行通过。
- 已产出首次提交并推送远程。

---

## Milestone 1：Workflow Foundation（当前）

### 目标

完善项目定位、架构、路线图与写作规范，确立内容建设的"宪法"。

### 预计时间

7 个 Sprint（7 天）。

### 交付内容

- `docs/00-roadmap/project-positioning.md`：项目背景、目标、使命、愿景、边界、原则、版本规划。
- `docs/00-roadmap/project-architecture.md`：完整目录职责与维护原则。
- `docs/00-roadmap/roadmap.md`：本文件，按 Milestone 推进。
- `docs/00-roadmap/writing-style.md`：全量写作规范（Markdown / 代码 / 图片 / 引用 / Prompt / Rule / Agent / 命名 / Commit）。
- 配套模板体系：章节、Prompt、Rule、Agent、Workflow、Case 模板。

### 完成标准

- 四份规范文档内容完整、可执行。
- 后续所有新增内容均遵循本阶段确立的规范。

---

## Milestone 2：基础认知

### 目标

建立 AI、大模型、Prompt、Agent 的基础认知体系，让读者具备理解后续工具的先验知识。

### 预计时间

7 个 Sprint（7 天）。

### 交付内容

- `docs/01-ai-basic/`：AI 开发时代、AI 与软件工程、AI 研发工作流总览。
- `docs/02-models/`：主流大模型对比、上下文窗口、Token 与经济性。
- `docs/10-prompts/`：Prompt 工程原理、结构化 Prompt 设计。
- `docs/11-agent/`：Agent 原理、能力边界、任务拆解与验收。

### 完成标准

- 每章包含 `index.md` 主文档与必要的配套文档。
- 每章至少一个实战案例与一组可复用 Prompt。

---

## Milestone 3：AI Coding 工具

### 目标

掌握主流 AI Coding 工具（OpenCode、Codex、Claude Code、CodeBuddy），能够根据场景选型并熟练使用。

### 预计时间

14 个 Sprint（14 天）。

### 交付内容

- `docs/03-opencode/`：安装、配置、项目级工作流、常用命令与 Prompt。
- `docs/04-codex/`：Codex 云端 Agent 的使用与工程接入。
- `docs/05-claude-code/`：Claude Code 的终端协作模式。
- `docs/06-codebuddy/`：CodeBuddy 的安装与使用。
- 每个工具配套 `checklist.md`（安装/配置/自检）与 `faq.md`（踩坑记录）。

### 完成标准

- 每个工具章节均可按文档从零完成一次真实开发任务。
- 产出"工具选型对比"章节，帮助读者决策。

---

## Milestone 4：全栈实战

### 目标

把 AI 工作流落地到真实技术栈，覆盖 Flutter、Android、Vue 三大方向。

### 预计时间

21 个 Sprint（21 天）。

### 交付内容

- `docs/12-flutter/`：Flutter + GetX + Clean Architecture 的 AI 工作流。
- `docs/13-android/`：Android / Kotlin 的 AI 工作流。
- `docs/14-vue/`：Vue 前端的 AI 工作流。
- 配套 `prompts/`、`rules/`、`agents/` 资产。

### 完成标准

- 每个技术栈完成至少一个端到端示例项目。
- 沉淀的 Prompt / Rule / Agent 在示例中得到验证。

---

## Milestone 5：MCP 与扩展

### 目标

掌握 MCP 原理，能够搭建 MCP 服务器并接入外部工具（GitHub、数据库、设计稿等）。

### 预计时间

7 个 Sprint（7 天）。

### 交付内容

- `docs/08-mcp/`：MCP 原理、协议、服务器搭建。
- `mcp/`：常用 MCP 配置与使用说明（github、git、figma、jira、sqlite、mysql 等）。

### 完成标准

- 至少完成一个自建 MCP 服务器的端到端案例。
- 常用 MCP 均有可复用的配置模板。

---

## Milestone 6：企业研发

### 目标

把个人工作流升级为企业级工程实践，覆盖团队协作、规范落地与研发流程。

### 预计时间

14 个 Sprint（14 天）。

### 交付内容

- `docs/15-enterprise/`：企业级 AI 研发流程、代码审查、发布与协作。
- 团队 Rules、审查清单、发布流程模板。

### 完成标准

- 沉淀一套可直接导入团队使用的企业研发工作流。

---

## Milestone 7：AI Team Workflow

### 目标

实现多 Agent 协作与 AI 驱动研发流程，探索"AI 团队"的组织方式。

### 预计时间

14 个 Sprint（14 天）。

### 交付内容

- `docs/16-case-study/`：多 Agent 协作案例。
- 跨 Agent 的任务编排、结果验收与知识回流机制。

### 完成标准

- 完整呈现一个多 Agent 协作研发的端到端案例。

---

## Milestone 8：稳定发布

### 目标

整体内容校验、修复、归档，发布 v1.0.0。

### 预计时间

7 个 Sprint（7 天）。

### 交付内容

- 全部章节完整性检查。
- 链接、图片、示例可运行性验证。
- 规范统一性复查。

### 完成标准

- 所有章节符合 `writing-style.md` 规范。
- 发布 v1.0.0 稳定版，进入长期维护模式。

---

## 对应目录

| Milestone | 目录 |
| --- | --- |
| Milestone 0 | 仓库根目录 |
| Milestone 1 | `docs/00-roadmap/`, `templates/` |
| Milestone 2 | `docs/01-ai-basic/`, `docs/02-models/`, `docs/10-prompts/`, `docs/11-agent/` |
| Milestone 3 | `docs/03-opencode/`, `docs/04-codex/`, `docs/05-claude-code/`, `docs/06-codebuddy/` |
| Milestone 4 | `docs/12-flutter/`, `docs/13-android/`, `docs/14-vue/` |
| Milestone 5 | `docs/08-mcp/`, `mcp/` |
| Milestone 6 | `docs/15-enterprise/` |
| Milestone 7 | `docs/16-case-study/` |
| Milestone 8 | 全仓库 |

## 更新规则

- 每完成一个 Milestone，勾选本文件对应状态，并同步更新 `CHANGELOG.md`。
- 新主题需先补充路线图，再开新章节。
