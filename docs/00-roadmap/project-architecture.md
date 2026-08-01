# Project Architecture

## 一级目录职责

| 目录 | 职责 |
| --- | --- |
| docs | 正式文档 |
| prompts | Prompt 库 |
| rules | Rules 库 |
| agents | Agent 库 |
| mcp | MCP |
| examples | 案例 |
| scripts | 脚本 |
| templates | 模板 |
| assets | 资源 |

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
