# 目录结构规范

> 定义项目顶层目录的职责边界，新增内容必须落在对应目录。

## 顶层目录

```
AI-FullStack-Workflow/
├── docs/          # 章节文档（教程 + 原理 + 案例）
├── prompts/       # Prompt 模板（按技术栈/场景分类）
├── rules/         # 编码规范与约束规则
├── agents/        # Agent 配置（每个 Agent 一个目录）
├── mcp/           # MCP 服务器配置
├── examples/      # 可运行示例项目
├── templates/     # 项目脚手架模板
├── scripts/       # 自动化脚本
└── assets/        # 静态资源（images 等）
```

## 新增内容原则

1. **目录先于内容**：新开主题时先建目录 + README，再填充内容。
2. **每个目录必须有 README.md**：一句话说明用途。
3. **禁止随意新增顶层目录**：内容必须归类到上述职责范围。
4. **文件命名**：小写短横线（`kebab-case`），如 `generate-page.md`。
5. **图片统一入 `assets/images/`**，禁止网络图片。

## 配套资源约定

每个技术章节（如 `docs/03-opencode/`）推荐结构：

```
docs/03-opencode/
├── index.md        # 原理与教程
├── prompts.md      # 本章 Prompt
├── checklist.md    # 操作检查清单
├── faq.md          # 常见问题
└── examples.md     # 案例
```

具体章节可按需增减，但 `index.md` 必须存在。
