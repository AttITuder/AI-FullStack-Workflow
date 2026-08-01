# Flutter 页面生成

> 使用 AI 生成符合项目架构规范的 Flutter 页面。

## Prompt

```text
你是一名资深 Flutter 工程师。

技术栈要求：
- Flutter 3.24
- GetX 状态管理
- Dio 网络请求
- Clean Architecture 分层

请生成【登录页面】。

要求：
1. 输出完整可运行代码
2. 按分层结构组织文件
3. 包含以下四部分：
   - Controller
   - Binding
   - View
   - Route

页面需求：
- 用户名输入框
- 密码输入框
- 登录按钮（加载中状态）
- 调用 /api/login 接口
- 成功跳转到首页
- 失败显示错误提示
```

## 使用说明

1. 将 `【】` 中的占位符替换为实际需求。
2. 根据项目实际技术栈调整 GetX / Dio / 架构选型。
3. 生成后对照项目 `rules/` 规范审查。

## 期望输出结构

```text
lib/
├── app/
│   └── routes/
├── modules/
│   └── login/
│       ├── controller/
│       ├── binding/
│       ├── view/
│       └── model/
└── services/
    └── api/
```

## 验收清单

- [ ] 代码可编译运行
- [ ] 遵循 Clean Architecture 分层
- [ ] 网络层走统一封装
- [ ] 状态管理使用 GetX
- [ ] 路由注册正确
