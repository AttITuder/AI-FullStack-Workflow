# Agent Workflow

> 本章节最重要的文章。把前面所有的概念落到"实际研发流程"上：Agent 如何驱动从需求到发布的全链路，以及各实际 Agent 的工作流长什么样。

---

## 1. Agent 基础 Workflow

最精简的 Agent 工作流，对应 `01-overview.md` 的端到端链路：

```text
用户需求
   ↓
Context（加载相关代码 / 文档）
   ↓
Prompt（系统提示词 + 本次任务说明）
   ↓
Rules（加载行为约束）
   ↓
Agent（进入执行闭环）
   ↓
Model（推理与规划）
   ↓
Tool / MCP（调用工具执行动作）
   ↓
Result（环境被改变）
   ↓
Validation（对照验收标准核对）
   ↓
下一步行动（继续 / 修正 / 完成 / 人工介入）
```

这条链说明一件事：**Agent 不是起点，而是编排者**。需求、Context、Prompt、Rules 先就位，Agent 才在它们的约束下驱动 Model 与 Tools 把意图变成结果，最后用 Validation 闭合。

## 2. 新功能开发 Agent Workflow

以"开发一个新功能"为例，展开基础 Workflow：

```text
需求（功能描述）
   ↓
Context：阅读相关模块，理解现状与架构
   ↓
Prompt：本次功能目标 + 约束（技术栈 / 范围）
   ↓
Rules：引用对应技术栈编码规范
   ↓
Agent 规划：拆解为页面 / 逻辑 / 接口等子任务
   ↓
逐个子任务执行：生成代码 → 构建验证
   ↓
Validation：可编译、结构合规、行为正确
   ↓
交付：变更清单 + 后续说明
```

关键控制点：

- Context 先读后写，避免风格漂移（见 `docs/10-prompts/`）。
- 大功能拆子任务分批执行，每批带验证（见 `03-agent-components.md` Planner/Validator）。
- 方案确认关卡放在自主执行之前。

## 3. Bug 修复 Agent Workflow

```text
现象（崩溃日志 / 异常描述）
   ↓
Context：定位相关代码
   ↓
Agent 规划：先定位根因，再给修复思路（禁止盲改）
   ↓
Executor：应用最小化修复
   ↓
Validation：原现象消失 + 相关测试无回归
   ↓
交付：根因说明 + 改动点
```

与功能开发的区别：Bug 修复强调**根因先行**——要求 Agent 先解释"为什么出错"再动手，防止"猜着改"的循环消耗。失败案例见 `docs/10-prompts/03-prompt-patterns.md` Debug Prompt。

## 4. 项目分析 Agent Workflow

```text
分析目标（"梳理 X 模块结构"）
   ↓
Context：扫描代码库
   ↓
Agent 规划：确定分析维度（分层 / 依赖 / 技术债）
   ↓
Executor：只读操作（不修改任何文件）
   ↓
Validator：输出是否覆盖既定维度
   ↓
交付：结构化分析报告
```

核心约束：**分析阶段只读不改**。"只读"既是权限边界也是安全底线——分析 Agent 不应在摸底时夹带修改（见 `02-agent-architecture.md` 第 8 节）。

## 5. Architecture Agent Workflow

结合 `agents/architect-agent/`（当前为规划中资产，以下为基于 Agent 模板的通用工作流示意）：

```text
架构问题（"评估是否引入 Repository 层"）
   ↓
Context：读取现有网络层与调用方
   ↓
Agent 规划：给出多个候选方案
   ↓
Planner：限定对比维度（成本 / 可维护性 / 影响面）
   ↓
Validator：方案是否覆盖所有约束
   ↓
交付：方案对比 + 推荐结论（待团队确认）
```

设计要点：架构 Agent 的价值在**多方案结构化对比**，而非直接给唯一答案。关键技巧是限定决策维度防止失焦（见 `docs/10-prompts/03-prompt-patterns.md` Architecture Prompt）。该 Agent 的具体 `prompt.md` / `workflow.md` 以 `agents/architect-agent/` 后续落地为准。

## 6. Android Agent Workflow

结合 `agents/android-agent/`（规划中资产，以下为通用示意）：

```text
Android 开发任务（页面生成 / Compose / 接口联调 / Bug 修复）
   ↓
Context：读取现有 Android 模块结构
   ↓
Prompt：技术栈约束（Jetpack / Compose 等）
   ↓
Agent 规划：按任务类型选择子流程
   ↓
Executor：生成 / 修改代码
   ↓
Validation：编译通过、遵循 Android 规范
   ↓
交付：变更清单 + 需执行的命令
```

Android Agent 的能力域（页面生成、Jetpack Compose、接口联调、Bug 修复、Code Review）已在其目录规划中定义；完整工作流随 `prompt.md` / `workflow.md` 建设而确定。

## 7. Flutter Agent Workflow

结合 `agents/flutter-agent/`——**本项目唯一完整落地的 Agent**，其 `workflow.md` 定义了标准流程：

```text
需求 → 分析 → 设计 → 生成 → 自检 → 交付
```

逐步展开（引用自 `agents/flutter-agent/workflow.md`）：

1. **需求分析**：明确页面 / 功能目标，确认技术栈与架构约束，输出任务拆解清单。
2. **设计**：确定数据模型、接口、状态管理与路由，输出文件结构。
3. **生成**：按模块生成代码（controller / binding / view / model），复用统一封装。
4. **自检**：对照验收清单——`[ ] 可编译运行` `[ ] 遵循 Clean Architecture` `[ ] 命名规范` `[ ] 无硬编码` `[ ] 错误处理完整`。
5. **交付**：说明新增 / 修改文件、需执行的命令（如 `flutter pub get`）、后续注意事项。

其 `prompt.md` 同步定义了角色（资深 Flutter 全栈工程师）、技术栈约束（Flutter 3.24+ / GetX / Dio / Clean Architecture）、工作原则（先理解再动手、遵循 `rules/`、可编译可运行）。这是其它 Agent 工作流设计的参考样板。

## 8. Git Agent Workflow

结合 `agents/git-agent/`（规划中资产，通用示意）：

```text
Git 任务（提交 / 分支 / 合并 / 冲突解决）
   ↓
Context：读取仓库状态、变更 diff、分支情况
   ↓
Rules：引用 git/ 提交与分支规范
   ↓
Agent 规划：生成提交信息或合并策略
   ↓
Executor：执行受控 Git 命令（危险操作需确认）
   ↓
Validation：提交信息合规、未误改文件、分支正确
   ↓
交付：操作说明
```

Git Agent 的权限边界尤其重要——`push` / `reset --hard` / 强制推送等危险命令必须 HITL 确认（见 `05-best-practice.md` 权限控制）。

## 9. Product Agent Workflow

结合 `agents/product-agent/`（规划中资产，通用示意）：

```text
产品需求原文
   ↓
Context：理解项目背景
   ↓
Agent 规划：拆解为开发任务清单
   ↓
Planner：每项含模块 / 依赖 / 复杂度 / 验收标准
   ↓
Validator：是否暴露需求中的模糊点
   ↓
交付：任务清单 + 待确认问题列表
```

Product Agent 最有价值的产出是"标注模糊点"——在开发前拦截返工（见 `docs/10-prompts/03-prompt-patterns.md` Product Prompt）。

## 10. Review Agent Workflow

结合 `agents/review-agent/`（规划中资产，通用示意）：

```text
待审查变更（diff / 文件）
   ↓
Context：读取变更与相关代码
   ↓
Rules：引用 review/ 审查规范
   ↓
Agent 规划：按维度审查（规范 / 逻辑 / 性能 / 安全）
   ↓
Observer + Validator：逐项给出问题 + 严重级别
   ↓
交付：分级问题清单（高优问题清零才放行）
```

Review Agent 是"轻执行"组合（Observer + Validator 为主，只读不改），详见 `03-agent-components.md` 组合表。AI 初审 + 人终审是推荐分工。

## 11. Test Agent Workflow

结合 `agents/test-agent/`（规划中资产，通用示意）：

```text
被测对象（模块 / 函数）
   ↓
Context：读取实现与现有测试写法
   ↓
Agent 规划：列出覆盖场景（正常 / 边界 / 异常）
   ↓
Executor：生成并运行测试
   ↓
Validator：测试通过 + 覆盖率达标
   ↓
交付：测试文件 + 运行结论
```

Test Agent 强调"与实现同环节完成"，不要攒到最后补测试（见 `docs/10-prompts/04-workflow.md` 测试验证环节）。

## 12. Release Agent Workflow

结合 `agents/release-agent/`（规划中资产，通用示意）：

```text
发布目标（版本 / 渠道）
   ↓
Context：读取构建配置、变更范围
   ↓
Rules：引用发布规范
   ↓
Agent 规划：构建 → 打包 → 提审 / 上线步骤
   ↓
Executor：调用构建 / 部署工具（危险操作 HITL）
   ↓
Validator：产物可运行、变更与需求一致
   ↓
交付：发布记录
```

Release Agent 是权限敏感型——部署动作必须人工确认，且全程留痕。

## 13. Multi-Agent Workflow

复杂研发可由多个专职 Agent 串联协作：

```text
Product Agent     需求拆解、暴露模糊点
   ↓
Architect Agent   方案设计、技术选型
   ↓
Developer Agent   编码实现（本项目中由 flutter-agent / android-agent 等承担）
   ↓
Test Agent        测试验证
   ↓
Review Agent      代码审查
   ↓
Release Agent     发布上线
```

说明：

- **Product / Architect / Test / Review / Release** 均为本项目规划中的 Agent（`agents/` 对应目录）。
- **Developer Agent** 在本项目 `agents/` 中**并不存在**——它在此处作为**概念示例**出现：在真实落地时，开发角色由具体技术栈 Agent（如 `flutter-agent`、`android-agent`）承担，而非一个独立的 `developer-agent`。
- Multi-Agent 的价值在于**职责单一、各司其职**，避免单个 Agent 什么都做导致提示词臃肿、能力稀释。协作通过"上一 Agent 的产出作为下一 Agent 的 Context"实现（见 `docs/10-prompts/04-workflow.md` 环节衔接）。

## 14. Agent + MCP Workflow

MCP 为 Agent 提供"手脚"，Workflow 因此跨越本地文件：

```text
Agent 规划动作（"构建并安装到设备验证"）
   ↓
MCP 解析：定位 ADB / Gradle Server
   ↓
Tool Calling：执行构建 + 安装 + 启动
   ↓
Observation：返回日志 / 运行结果
   ↓
Validator：是否通过
```

设计要点：

- 写 workflow 时**假设工具可用**，不描述工具实现细节。
- 工具调用的安全由 Rules + 权限边界约束（生产只读、敏感操作确认）。
- 详见 `docs/08-mcp/`。

## 15. Agent + Rules Workflow

Rules 是 Agent 工作流的"纪律引擎"：

```text
Agent 启动
   ↓
加载 Rules（编码 / 审查 / 提交规范）
   ↓
执行中：每个动作对照 Rules 自检
   ↓
Validator：产出是否符合 Rules
   ↓
不合规 → 修正后重试
```

价值：

- Agent 不必内联重复规范，引用即可。
- Rules 更新（如新增命名规范）后，所有 Agent 自动受益。
- 当某条约束反复出现在多个 Agent 中，应下沉为 Rules（问题上升机制，见 `docs/10-prompts/05-best-practice.md`）。

## 16. Agent + Prompt Workflow

Prompt 是每次工作流的"任务入口"：

```text
系统提示词（prompt.md，长期角色）
   + 任务提示词（本次目标，单次）
   ↓
Agent 进入闭环
```

协作要点：

- 高频任务把成熟任务 Prompt 沉淀到 `prompts/` 或升级为 Agent 配置（见 `docs/10-prompts/05-best-practice.md` 升级路径）。
- 模糊任务先澄清再执行——任务级 HITL。
- 长任务用"接力 Prompt"跨会话恢复 Context（见 `04-workflow.md` 上文会话管理思路，亦见 `docs/10-prompts/04-workflow.md`）。

## 17. 完整 AI FullStack Workflow

把以上全部汇入一条覆盖研发全生命周期的链路：

```text
需求
   ↓
Prompt（任务意图层：这次要做什么）
   ↓
Rules（行为约束层：长期应该怎么做）
   ↓
Agent（执行层：编排闭环）
   ↓
Model（推理层：规划与生成）
   ↓
MCP / Tools（工具层：构建 / 查询 / 部署）
   ↓
执行（读写代码、跑命令）
   ↓
测试（Test Agent / 自动化用例）
   ↓
Review（Review Agent / 人工审查）
   ↓
Release（Release Agent / 上线）
```

这就是 AI FullStack Workflow 的"全景图"：六层能力（Model / Prompt / Rules / Agent / MCP / Workflow）被组织成一条可复制、可协作、可验收的研发流水线。每一层都有独立章节承载方法论（`docs/02-models/`、`docs/09-rules/`、`docs/10-prompts/`、`docs/11-agent/`、`docs/08-mcp/`），而 `agents/` 目录是这条流水线中"执行层"的实际资产。

> 提醒：本图是**逻辑编排**，不表示工具调用顺序固定不变——实际中 Agent 会在执行与测试间反复迭代，Review 的发现也会回流到修改。Workflow 的价值在于"有稳定可复现的主干"，而非"每一步只能走一次"。

## 总结

- 基础 Workflow：需求 → Context → Prompt → Rules → Agent → Model → Tool/MCP → Result → Validation → 下一步。
- 功能开发：大任务拆子任务分批 + 方案确认关卡；Bug 修复：根因先行、最小化修复。
- 项目分析：只读不改，输出结构化报告。
- 各实际 Agent 工作流：flutter-agent 已完整落地（需求→分析→设计→生成→自检→交付），android / architect / git / product / review / test / release 为规划中资产，本文给出基于 Agent 模板的通用示意。
- Multi-Agent：Product→Architect→Developer(概念示例)→Test→Review→Release，按职责串联，产出即下一环 Context。
- 三层组合：MCP 给手脚、Rules 给纪律、Prompt 给入口。
- 完整链路：需求→Prompt→Rules→Agent→Model→MCP/Tools→执行→测试→Review→Release，是六层能力的研发流水线。

## 参考资料

- 上一节：`03-agent-components.md`
- 下一节：`05-best-practice.md`
- 工具层：`docs/08-mcp/`
- 约束层：`docs/09-rules/`
- 意图层：`docs/10-prompts/04-workflow.md`
- 实际资产：`agents/flutter-agent/`
