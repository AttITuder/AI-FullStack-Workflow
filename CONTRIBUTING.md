# 贡献指南

感谢你愿意参与本项目。请阅读以下约定，确保协作顺畅。

## 目录结构

```
AI-FullStack-Workflow/
├── docs/         # 文档
├── prompts/      # Prompt 模板
├── rules/        # 编码规范与规则
├── agents/       # Agent 配置
├── mcp/          # MCP 服务器配置
├── examples/     # 示例项目
├── templates/    # 模板文件
├── scripts/      # 自动化脚本
└── assets/       # 静态资源
```

## 约定

1. **每个目录都必须有 README.md**，用一句话说明该目录用途。
2. 新增内容放到对应语义的目录，不要随意新建顶层目录。
3. 修改涉及用户可见变更时，更新 `CHANGELOG.md`。
4. 提交信息遵循 [Conventional Commits](https://www.conventionalcommits.org/zh-hans/) 规范，如 `feat:`, `fix:`, `docs:`, `refactor:`, `test:`。

## 提交流程

1. 从 `main` 分支创建自己的分支：`git checkout -b feat/your-feature`
2. 完成变更后运行 `git status` 确认仅包含预期文件
3. 提交并推送，创建 Pull Request
4. PR 需通过 CI 检查后方可合并

## 提交信息示例

```
feat: 添加 flutter prompt 模板
fix: 修正 roadmap 文档链接
docs: 更新 README 目录结构说明
```

## 问题与讨论

- Bug 或功能请求请使用 [Issue 模板](.github/ISSUE_TEMPLATE/) 提交
- 其他讨论可发起的 Discussions 板块
