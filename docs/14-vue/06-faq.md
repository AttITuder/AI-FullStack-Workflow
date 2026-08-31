# Vue AI 开发 FAQ

> 常见问题速查，覆盖 Vue3 / Composition API / TypeScript / Vite / Pinia / Router / 组件化 / 工程化 / AI 生成 / Prompt / Rules / Agent / MCP / Debug / 性能 / 测试 / 企业项目。按 `Q + ### A` 格式组织，方便快速检索。

---

## Q1

本章节是 Vue 教程吗？和官方开发文档有什么区别？

### A

不是。本章节讲的是"如何在 AI FullStack Workflow 中工程化地组织 Vue 前端开发"——如何用 Model、Prompt、Rules、Agent、MCP 提升效率与质量。官方文档讲"Vue 怎么做"，本章讲"AI 怎么帮你把 Vue 前端做好、怎么沉淀可复用工作流"。

## Q2

Vue 在 AI FullStack Workflow 中处于什么位置？

### A

位于全栈实战层（`docs/14-vue/`），与 Flutter、Android 同属全栈实战，前面是基础认知、工具实操、MCP / Rules / Prompt / Agent，后面是企业研发。它是前面所有方法论在 Vue 前端技术栈上的**工程化集成与落地示范**。

## Q3

AI 走 Vue 开发还是 Vue2 开发？

### A

现代项目应默认面向 Vue3 组合式写法（Composition API）。存量 Vue2 项目可让 AI 在 Vue2 语法下产出，或辅助渐进迁移到 Vue3。关键是与项目实际版本一致，不混用。

## Q4

Composition API 和 Options API，AI 更推荐哪个？

### A

Composition API（组合式）是 Vue3 推荐，逻辑聚合、可复用、可测试性强，对 AI 也更友好。Options API 存在于存量项目。AI 生成时应与项目既有写法保持一致，新项目优先 Composition API。

## Q5

AI 生成 Vue 代码最常见的失败点是什么？

### A

路由未注册、响应式丢失（解构 / 赋值）、状态边界混乱（滥用全局 store）、TS 类型用 any、不遵守项目 CSS / UI 库方案。规避方法：提供充足 Context、用 Rules 约束、用验收清单核对、lint / build / 测试验证。

## Q6

`ref` 和 `reactive` 什么时候用？

### A

`ref` 用于基础类型与单独的值，模板中自动解包；`reactive` 用于对象。规则：能用 `ref` 就用 `ref`，避免把响应式对象整体解构（会丢失响应性）。

## Q7

响应式丢失是怎么回事？

### A

把 `reactive` 对象的属性解构出来、或对 `ref` 的 `.value` 重新赋值后，脱离了响应式链路，界面就不再更新。规避：用 `toRefs`、`computed` 或保持引用。

## Q8

`computed` 和 `watch` 怎么区分？

### A

`computed` 用于**派生数据**（从已有状态计算，缓存，只读使用）；`watch` 用于**副作用**（状态变化时执行操作，如请求）。用错会导致多余渲染或逻辑混乱。

## Q9

Vue 状态管理用 Pinia 还是 Vuex？

### A

Vue3 项目推荐 Pinia（简约、类型安全、基于 Composition API）。Vuex 是 Vue2 经典方案，存量项目升级时可辅助迁移。**不把某一方案视为唯一答案**，按项目现状选择。

## Q10

什么时候用全局 store，什么时候用局部状态？

### A

仅属于单个组件 / 页面的状态用局部 `ref` / `reactive`；跨组件 / 跨页面共享的状态（用户、权限、主题）才放 Pinia。原则：能用局部就不用全局，避免滥用 store 增加复杂度。

## Q11

AI 生成页面时会漏掉路由吗？

### A

可能。规避方法是把"同步产出路由并懒加载"写入验收清单，Review 时重点核对。新页面只有注册路由后才能被访问。

## Q12

Vue Router 的懒加载为什么重要？

### A

用 `() => import(...)` 按需加载路由组件，可显著减小首屏包体积、加快加载。这是 AI 生成页面时的重要约定，也是性能优化基础。

## Q13

AI 怎么理解一个 Vue 项目的结构？

### A

通过读取目录结构、路由配置、`package.json`、关键组件，识别技术栈（Vue3 / TS / Pinia / UI 库 / CSS 方案）、数据流与组件组织，再对齐风格与架构，避免"盲写"。

## Q14

TypeScript 和 AI 协作时要注意什么？

### A

为 props / emits 与接口数据定义类型，避免 `any` 泛滥，合理用泛型，开启 `tsconfig` 的 `strict`。类型越明确，AI 与人的理解越一致，运行期错误也越少。

## Q15

AI 生成的 UI 还原度高吗？

### A

取决于 Context：提供设计稿（可结合 MCP 读 Figma）、尺寸、颜色、间距与交互要求，还原度会显著提高。Context 越细还原度越高，还需人工视觉比对调整。

## Q16

UI 组件库怎么选，会影响 AI 吗？

### A

会。Element Plus、Ant Design Vue、Naive UI、Vant 等使用方式不同。AI 生成时应按项目选定的组件库命名与写法产出，保持一致，避免引入未知依赖。

## Q17

CSS 方案（Tailwind / SCSS / CSS Modules）会怎么影响 AI？

### A

不同 CSS 方案的写法差异大。AI 生成样式时应遵循项目选定的方案与命名，不能混用。在 Prompt / Rules 中声明 CSS 方案能避免样式风格漂移。

## Q18

AI 生成代码后，最快的验证手段是什么？

### A

跑 `npm run lint` 检查规范、`vue-tsc` 做类型检查、`npm run build` 验证构建、`npm run test` 跑测试。构建与 lint 是 AI 生成后最先暴露问题的反馈闭环。

## Q19

遇到 Vue 组件不更新，AI 能帮什么？

### A

AI 会检查响应式丢失、`key` 缺失、事件绑定、生命周期时机等常见原因，结合代码给出根因与修复。提供相关组件代码与期望行为能提高定位准确性。

## Q20

AI 辅助 Debug 时应该怎么提问？

### A

提供报错信息、相关代码、复现步骤、浏览器与网络请求。让 AI"先定位根因再给方案"，避免片段的"盲猜"。

## Q21

Vue 性能优化，AI 一般给什么建议？

### A

基于测量数据：路由 / 组件懒加载、合理用 `computed`、列表 `key` 与虚拟滚动、请求并发与缓存、代码拆分与 Tree-shaking。改造后复测确认收益。

## Q22

AI 能生成 Vue 测试吗？

### A

能。可生成单元测试（composables、store、工具函数）与组件测试（渲染、事件、props / emits）。跑 `npm run test` 验证，测试是信任 AI 产出的前提。

## Q23

Prompt 在 Vue 开发中的角色是什么？

### A

把需求翻译成明确、带约束的任务。好的 Prompt 含角色设定、技术栈约束、明确任务、输出要求与验收清单，用 `【】` 占位符复用模板（`prompts/vue/`）。

## Q24

Rules 在 Vue 开发中的角色是什么？

### A

作为 ID 与开发的"行为准则"，约束 Composition API 写法、TS 类型、组件命名与结构、状态管理，保证产出风格一致、质量可预期。Rules 要可执行、可检查，并同步给 Prompt 与 Agent 引用。

## Q25

Vue 前端开发用哪个 Agent？

### A

项目中 `agents/` 沉淀了架构（`architect-agent`）、Review（`review-agent`）、测试（`test-agent`）等可复用 Agent，可在 Vue 工程中组合使用：架构 Agent 理清项目、Review Agent 审查、Test Agent 辅助测试。

## Q26

Agent 和直接问 Chat AI 有什么区别？

### A

Agent 把"身份、技术栈、工作原则、流程"固化成资产，一次加载长期复用，命令式委派即可；Chat AI 每次都要重新描述一切，且缺少流程纪律。生产更稳定、更可复制。

## Q27

MCP 在 Vue 开发中能干什么？

### A

连接外部能力：Figma 读设计稿辅助 UI 还原、接口 / 规范文档作为生成 Context、GitHub / Git 管理变更、数据 / 数据库联调参考，让 AI 触达"真实世界"数据。

## Q28

MCP 使用有什么安全注意？

### A

密钥一律走环境变量，不写进仓库；按需启用避免信息过载；涉及敏感操作时谨慎授权。配置参考 `mcp/` 与 `docs/08-mcp/`。

## Q29

AI 生成组件时，接口（props / emits）容易设计得不好吗？

### A

有可能。规避方式是让 AI"先明确 props / emits 再写实现"，保持接口单一职责、命名清晰，Review 时核对。好的接口是组件可复用、可 AI 维护的基础。

## Q30

AI 会默认把代码生成到错误的位置吗？

### A

可能。规避方法是提供目录结构与模块职责作为 Context，让 AI"先理解归属再写"，并在 Review 时检查代码落在正确的位置（views / components / stores / api）。

## Q31

前端工程化（Vite / ESLint / Prettier）对 AI 有什么影响？

### A

工程化提供了验证闭环：Vite 构建暴露编译错误，ESLint / Prettier 约束风格。AI 生成代码后跑这些校验，能自动纠正大部分质量问题，是"信任 AI 产出"的基础。

## Q32

企业 Vue 项目怎么落地 AI 工作流？

### A

沉淀统一资产（`prompts/vue/`、`rules/vue/`、`agents/`、`mcp/`），建立构建（Vite）+ 测试 + Code Review + 人工拍板 + 发布（构建产物 / 环境变量 / 部署）的质量闭环，从个人提效逐步演进为稳定可复现的团队流水线。

## Q33

AI 生成代码时，会不会强制推一种技术选型？

### A

有可能，AI 有偏好流行方案的倾向。应通过 Project Rules 与 Prompt 把"本项目技术栈"作为硬约束，Review 时校验是否符合项目实际，避免强推 Pinia / Vuex、UI 库或 CSS 方案之外的选择。
