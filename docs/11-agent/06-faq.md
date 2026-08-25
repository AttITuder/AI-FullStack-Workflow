# Agent FAQ

> 常见问题速查。按概念、架构、组件、协作、实际 Agent、企业落地六类组织。新问题被解答后，回填到对应分类。

---

## 概念与定义

## Q1: Agent 到底是什么？

### A

Agent 是能**自主感知环境、推理规划、调用工具并采取行动**以完成目标的 AI 系统。它不是聊天机器人（只给答案），也不是 Model 本身（只做推理），而是把模型、上下文、规则、工具组织成执行闭环的机制。详见 `01-overview.md` 第 1 节。

## Q2: AI Agent 和 Coding Agent 有什么区别？

### A

Coding Agent 是 Agent 在软件研发领域的**一种具体形态**——当"环境"是代码库、"动作"是编码与命令、"目标"是工程任务时，Agent 就是 Coding Agent。通用概念（感知—推理—行动）在所有领域成立，Coding Agent 只是实例化之一。见 `01-overview.md` 第 5 节。

## Q3: Agent 和 Chat AI 的根本区别是什么？

### A

Chat AI 输出文本答案，人自己落地；Agent 直接改变环境（改文件、跑命令、查数据），并观察结果自我纠正。区别在"是否行动"与"是否多步自主"。对照表见 `01-overview.md` 第 1 节。

## Q4: Autonomous Agent 是什么？

### A

强调"自主性"的 Agent：给定目标后自行拆解步骤、连续调用工具、自我纠正直至达成，尽量少依赖逐步人工指令。自主是光谱——多数工程 Agent 在方案确认、最终 Review 仍需人介入（HITL）。见 `01-overview.md` 第 1 节。

## Q5: Agent 是不是就是大模型？

### A

不是。Model 是 Agent 的推理引擎，没有 Agent 编排，Model 只是会聊天的文本生成器；没有 Model，Agent 无法理解意图。两者是引擎与整车的关系。见 `01-overview.md` 第 6 节。

## Q6: Agent 演进经历了哪些阶段？

### A

Chat AI → Copilot → Coding Agent → Agent Workflow。本质是把人的注意力从逐步操作中解放出来，最终形成稳定可复现的研发流程。见 `01-overview.md` 第 2 节。

## Agent Loop 与架构

## Q7: Agent Loop 是什么？

### A

Agent 的运行闭环：Observe（观察）→ Think（推理）→ Plan（规划）→ Act（执行）→ Observe（再观察），循环至目标达成。工程化实例见 flutter-agent `workflow.md`：需求→分析→设计→生成→自检→交付。见 `01-overview.md` 第 3 节、`02-agent-architecture.md` 第 2 节。

## Q8: Agent Loop 和普通脚本循环有何不同？

### A

脚本循环是写死的，不观察结果；Agent Loop 每步动作后**观察实际结果**再决策下一步，具备自我纠正能力。这是 Agent 区别于自动化脚本的根本。见 `02-agent-architecture.md` 第 2、10 节。

## Q9: Agent 的基本架构包含哪些部件？

### A

概念模型含十类角色：User、Agent、Model、Prompt、Rules、Context、Memory、Tools、MCP、Environment。这是概念模型而非具体实现，不同 Coding Agent 工具内部实现各异。见 `02-agent-architecture.md` 第 1 节。

## Q10: Agent Architecture 和传统应用架构的根本区别？

### A

传统架构正确由控制流保证（确定性）；Agent 架构正确由**验证闭环 + 人**保证（概率性、动态规划）。因此 Agent 系统必须把验证与人当作一等公民。见 `02-agent-architecture.md` 第 10 节。

## Q11: Agent 的决策过程分几级？

### A

三级：目标级（是否做、是否需澄清）、规划级（分几步、每步什么）、执行级（调哪个 Tool、参数如何）。风险靠验证闭环 + HITL 控制。见 `02-agent-architecture.md` 第 3 节。

## Q12: Agent 的上下文管理有哪些原则？

### A

按需加载（先读相关模块而非全量粘贴）、控制窗口（删冗余保关键）、阶段归档（长任务固化事实）、接力恢复（跨会话用工件摘要重注）。见 `02-agent-architecture.md` 第 4 节、`05-best-practice.md` 第 3 节。

## Q13: Agent 的状态管理指什么？

### A

两类状态：会话内（本次任务进度，结束即消散）与跨会话（偏好、约定、历史决策，应沉淀到 Memory/Rules）。状态管理保证可恢复、可观测、可回放。见 `02-agent-architecture.md` 第 6 节。

## Q14: Agent 的记忆机制如何实现？

### A

本项目不另设存储，用三层工程承担：稳定偏好→`rules/` 或 `prompt.md`；可复用做法→`prompts/` 或 `workflow.md`；一次性经验→会话归档后择优上升。见 `02-agent-architecture.md` 第 7 节、`03-agent-components.md` 第 5 节。

## 核心组件

## Q15: Planner、Executor、Observer、Validator 各负责什么？

### A

- **Planner**：把目标拆为执行计划（推理→规划段）。
- **Executor**：调用 Tools 改变环境（Tool Calling 段）。
- **Observer**：每步后收集结果判断是否按预期（Observation 段）。
- **Validator**：对照验收标准判断任务是否真完成。

四者构成执行闭环。见 `03-agent-components.md` 第 8–11 节。

## Q16: Human-in-the-loop 是组件吗？

### A

是贯穿全程的"元组件"，不是某个阶段。在目标级、方案级、风险级、验收级四类节点让人介入，是可控性的根本保证。见 `03-agent-components.md` 第 12 节、`05-best-practice.md` 第 9 节。

## Q17: 是否每个 Agent 都必须具备全部组件？

### A

不必。轻量对话 Agent 只需 Model+Prompt+Context；审查 Agent 偏 Observer+Validator（只读）；编码 Agent 才需全组件。按"需要感知什么、改变什么、验证什么"选配。见 `03-agent-components.md` 组合表。

## Q18: Context 和 Memory 有什么区别？

### A

Context 是**本次任务**可见的信息（代码、会话、检索）；Memory 是**跨会话**沉淀的经验与偏好。Context 随任务结束消散，Memory 长期保留。见 `02-agent-architecture.md` 第 4、7 节。

## 与相邻层的关系

## Q19: Agent 与 Model 的关系？

### A

Model 是推理引擎，Agent 是编排者。没有 Model 无法理解规划，没有 Agent 模型只是文本生成器。Model 决定能力上限，Agent 决定能力是否被有效组织。见 `01-overview.md` 第 6 节。

## Q20: Agent 与 Prompt 的关系？

### A

Prompt 是交给 Agent 的"任务委托书"：系统提示词（prompt.md）长期定义角色，任务提示词单次给定目标。Agent 是 Prompt 的宿主与执行者；高频任务 Prompt 应升级为 Agent 配置。见 `01-overview.md` 第 7 节。

## Q21: Agent 与 Rules 的关系？

### A

Rules 是 Agent 长期遵守的纪律，Agent 引用而非重复实现。一句话：Prompt 管一次任务，Rules 管长期行为，Agent 在约束下执行。见 `01-overview.md` 第 8 节、`05-best-practice.md` 第 5 节。

## Q22: Agent 与 MCP 的关系？

### A

MCP 为 Agent 提供"手脚"——标准化连接外部工具与数据。Agent 决定做什么动作，MCP 提供动作怎么实现。写 workflow 时假设工具可用即可，安全由 Rules 兜底。见 `01-overview.md` 第 9 节、`04-workflow.md` 第 14 节。

## Q23: Agent 与 Tool 的关系？

### A

Tools 是 Agent 可执行的**具体动作单元**（读文件、跑命令、调 API）；MCP 是 Tools 的协议与来源之一。Agent 能力上限 = 可访问 Tools 集合。见 `01-overview.md` 第 10 节、`03-agent-components.md` 第 6 节。

## 安全与成本

## Q24: Agent 的权限应该如何设计？

### A

最小权限 + 四层边界：文件（允许改的目录）、命令（执行白名单）、环境（只读生产 / 可写开发）、数据（不读密钥）。危险操作（删除、push、部署）必须人工确认。见 `02-agent-architecture.md` 第 8 节、`05-best-practice.md` 第 8 节。

## Q25: Agent 有哪些安全风险？

### A

错误调用工具删文件、推错分支、触达生产环境；权限过大放大破坏面；数据泄露（读取密钥外传）。对策：最小权限、危险操作 HITL、数据脱敏、全程日志。见 `01-overview.md` 第 13 节、`05-best-practice.md` 第 8 节。

## Q26: Agent 的成本如何控制？

### A

控上下文（删冗余、按需加载）、控循环（限制 Agent Loop 最大步数）、选模型（重任务强模型、轻任务小模型）、批处理相似子任务、设预算上限。多步自主执行消耗远高于单次问答。见 `05-best-practice.md` 第 13 节。

## Q27: Agent 执行失败了怎么办？

### A

错误恢复五策略：重试（换参数）、回退（撤销上步）、换路径（重新规划）、求助（HITL）、终止（不可恢复时安全停止）。前提：每步动作可回滚、Observation 返回失败信息。见 `02-agent-architecture.md` 第 9 节。

## Q28: Agent 应该重试多少次？

### A

没有固定次数，工程原则是**连续失败即升级为人**，避免无限重试烧成本。配合成本预算上限，超限告警。见 `05-best-practice.md` 第 10、13 节。

## Q29: 如何验证 Agent 的输出？

### A

"完成"由验证定义，不由 Agent 宣布。验收清单来自真实踩坑，Validator 对照逐项核对，AI 自检 + 人终审双重使用；高风险产出必须人工验收放行。见 `05-best-practice.md` 第 11 节。

## Multi-Agent 与落地

## Q30: 什么是 Multi-Agent Workflow？

### A

多个专职 Agent 串联：Product→Architect→Developer→Test→Review→Release，上一 Agent 产出即下一环 Context。注意 Developer Agent 在本项目是**概念示例**，开发职责由 flutter-agent / android-agent 等技术栈 Agent 承担。见 `04-workflow.md` 第 13 节。

## Q31: Multi-Agent 设计要注意什么？

### A

职责单一、产出即 Context、明确交接协议（输入输出格式）、避免环形依赖（流程应是 DAG）、开发角色勿虚构独立 Agent。见 `05-best-practice.md` 第 14 节。

## Q32: Flutter Agent 工作流是怎样的？

### A

本项目唯一完整落地的 Agent（`agents/flutter-agent/`），流程：需求→分析→设计→生成→自检→交付。其 `prompt.md` 定义角色与技术栈约束（Flutter 3.24+ / GetX / Dio / Clean Architecture），`workflow.md` 定义自检清单。是其它 Agent 的参考样板。见 `04-workflow.md` 第 7 节。

## Q33: Android / Architect / Git / Product / Review / Test / Release Agent 现在能用吗？

### A

这些 Agent 目录已在 `agents/` 建立，但 `prompt.md` / `workflow.md` 尚在建设中（README 标注"配置待补充"）。本文给出基于 Agent 模板的**通用工作流示意**，具体资产以 `agents/` 后续落地为准；文档不虚构其内部实现。见 `04-workflow.md` 第 5–12 节。

## Q34: Agent 企业落地需要注意什么？

### A

- **安全合规**：最小权限、数据脱敏、生产只读、危险操作 HITL。
- **可控可观测**：日志留痕、变更清单、可回滚。
- **团队统一**：把成熟 Agent / Prompt / Rules 沉淀为库，成员一致体验。
- **成本治理**：预算上限、模型分级、定期复盘一次通过率。
- **长期维护**：实测入库、反馈驱动、问题上升为 Rules。

见 `05-best-practice.md` 各节与 `docs/00-roadmap/project-positioning.md`。

## Q35: Agent 如何长期维护而不腐化？

### A

四条：实测入库（新配置跑通再入）；反馈驱动（不好用小步改）；问题上升（反复约束下沉 Rules）；版本走 Git、废弃有仪式（过期明确删除 / 归档，README 同步）。核心是把它当工程资产而非一次性提示词。见 `05-best-practice.md` 第 17–19 节。

---

## 没找到答案？

- 概念与边界：`01-overview.md`
- 架构与运行：`02-agent-architecture.md`
- 组件协作：`03-agent-components.md`
- 研发工作流：`04-workflow.md`
- 工程化落地：`05-best-practice.md`
