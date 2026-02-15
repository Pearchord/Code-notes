

# PocketChat 项目：从零开始的阶段性学习计划

---

## 技术栈总览（你将要学的东西）

通过分析项目，以下是完整的技术栈地图：

| 层级         | 技术                                        | 角色          |
| ---------- | ----------------------------------------- | ----------- |
| **基础三件套**  | HTML / CSS / JavaScript                   | 网页基石        |
| **类型系统**   | TypeScript                                | JS 的类型增强    |
| **前端框架**   | Vue 3 (Composition API, `<script setup>`) | UI 框架       |
| **构建工具**   | Vite                                      | 开发服务器 + 打包  |
| **路由**     | Vue Router                                | 页面导航        |
| **状态管理**   | Pinia + 持久化插件                             | 全局状态        |
| **服务端状态**  | TanStack Vue Query                        | 数据请求/缓存     |
| **样式**     | TailwindCSS + SCSS                        | 样式系统        |
| **UI 组件库** | Element Plus + Naive UI                   | 现成 UI 组件    |
| **工具库**    | VueUse / lodash-es / Zod                  | 实用工具        |
| **HTTP**   | Axios + PocketBase SDK                    | 网络请求        |
| **后端**     | PocketBase (BaaS) + pb_hooks (JS)         | 数据库/认证/实时订阅 |
| **PWA**    | vite-plugin-pwa + Workbox                 | 离线体验        |

---

## 阶段一：地基 — HTML + CSS + JS 最小必要知识（约 5-7 天）

> **目标**：不求精通，只求能看懂 `.vue` 文件中的模板和样式部分。

### 任务 1.1：HTML 基础（1 天）

**学什么**：标签、属性、DOM 树的概念

**实战练习**：
1. 用纯 HTML 写一个聊天气泡界面（不需要样式），包含：`<div>`、`<span>`、`<img>`、`<input>`、`<button>`、`<ul>`/`<li>`
2. 理解 `class` 和 `id` 属性的作用

**对应项目文件**：打开 vue3/index.html — 这是整个前端的入口 HTML

**推荐资源**：[MDN HTML 入门](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML)

---

### 任务 1.2：CSS 基础 + TailwindCSS 入门（2 天）

**学什么**：选择器、盒模型、Flexbox 布局、CSS 变量

**实战练习**：
1. **Day 1**：用纯 CSS 把任务 1.1 的聊天气泡变好看 — 学 `padding`、`margin`、`border-radius`、`background-color`、`display: flex`
2. **Day 2**：用 TailwindCSS 重写同样的界面 — 学 `p-4`、`m-2`、`rounded-lg`、`bg-blue-500`、`flex`、`gap-2`

**对应项目文件**：
- vue3/tailwind.config.js — Tailwind 配置，定义了自定义颜色
- vue3/src/assets/styles/ — 项目的 SCSS 样式文件

**关键概念**：项目用 **CSS 变量**（`var(--color-background)`）实现暗色模式切换。在 tailwind.config.js 中能看到这些映射。

**推荐资源**：
- [Flexbox 青蛙游戏](https://flexboxfroggy.com/#zh-cn)（30 分钟学会 Flex 布局）
- [TailwindCSS 官方文档](https://tailwindcss.com/docs)（当字典查）

---

### 任务 1.3：JavaScript 核心（2-3 天）

**学什么**（只学这些，不多不少）：

| Day | 主题 | 关键内容 |
|---|---|---|
| Day 1 | 变量与数据类型 | `let`/`const`、字符串、数组、对象 |
| Day 1 | 函数 | 箭头函数 `() => {}`、参数、返回值 |
| Day 2 | 数组操作 | `.map()`、`.filter()`、`.find()`、`.flatMap()`、展开运算符 `...` |
| Day 2 | 异步 | `async/await`、`Promise`（PocketBase API 全是异步的） |
| Day 3 | 模块系统 | `import`/`export`（ES Module，项目中每个文件都在用） |
| Day 3 | 解构赋值 | `const { name, age } = user`、`const [first, ...rest] = arr` |

**实战练习**：
```js
// 模拟：从"后端"获取消息列表，过滤出某用户的消息，渲染到页面
const messages = [
  { id: 1, user: 'Alice', text: 'Hello!' },
  { id: 2, user: 'Bob', text: 'Hi there!' },
  { id: 3, user: 'Alice', text: 'How are you?' },
]
const aliceMessages = messages.filter(m => m.user === 'Alice')
console.log(aliceMessages) // 试一试
```

**推荐资源**：[现代 JavaScript 教程](https://zh.javascript.info/)（前 6 章即可）

---

### 任务 1.4：TypeScript 最小必要知识（1 天）

**学什么**：类型注解、接口/类型别名、泛型基础

**实战练习**：
```ts
// 给消息定义类型 — 这就是项目中 types/ 目录在做的事
interface Message {
  id: string
  text: string
  userId: string
  created: string
}

function getMessageById(messages: Message[], id: string): Message | undefined {
  return messages.find(m => m.id === id)
}
```

**对应项目文件**：
- vue3/src/types/index.d.ts — 项目类型定义
- vue3/src/lib/pocketbase/pocketbase-types.ts — 从 PocketBase schema 自动生成的类型

---

## 阶段二：Vue 3 核心 — 理解项目的骨架（约 5-7 天）

> **目标**：能读懂项目中任何一个 `.vue` 文件的结构

### 任务 2.1：Vue 3 基础（2 天）

**学什么**：
- `.vue` 单文件组件的三部分：`<script setup>`、`<template>`、`<style>`
- 响应式：`ref()`、`reactive()`、`computed()`
- 模板语法：`{{ }}`、`v-if`、`v-for`、`v-bind`（`:`）、`v-on`（`@`）
- 组件 Props 和 Emits

**实战练习**：写一个 `ChatBubble.vue` 组件
```vue
<script setup lang="ts">
const props = defineProps<{
  userName: string
  message: string
  isMine: boolean
}>()
</script>

<template>
  <div :class="isMine ? 'bg-blue-500 text-white ml-auto' : 'bg-gray-200 mr-auto'"
       class="max-w-xs rounded-lg p-3">
    <div class="text-xs font-bold">{{ userName }}</div>
    <div>{{ message }}</div>
  </div>
</template>
```

**对应项目文件**：打开 vue3/src/App.vue — 感受真实组件的结构

**推荐资源**：[Vue 3 官方教程](https://cn.vuejs.org/tutorial/)（交互式的，非常好）

---

### 任务 2.2：Vue Router — 页面是怎么切换的（1 天）

**学什么**：路由配置、`<RouterView>`、`<RouterLink>`、路由守卫、嵌套路由

**对应项目文件**：
- vue3/src/router.ts — 路由定义。项目用嵌套路由：`LayoutSimple` 是外壳，`ChatHome`/`LoginPage`/`ImageSelectPage` 等是子页面
- vue3/src/config/router.ts — 路由常量字典 `routerDict`

**实战练习**：
1. 读 router.ts，画出路由结构图（哪个 URL 对应哪个页面组件）
2. 在本地跑起项目（`cd vue3 && pnpm install && pnpm dev`），在浏览器中切换页面，对照代码

---

### 任务 2.3：Pinia — 全局状态管理（1 天）

**学什么**：`defineStore()`、`state`、`actions`、`getters`、持久化

**对应项目文件**（按难度递增）：
- vue3/src/stores/auth.ts — 认证状态（最关键）
- vue3/src/stores/i18n.ts — 国际化状态
- vue3/src/stores/setting-state.ts — 设置状态

**实战练习**：
1. 阅读 `auth.ts`，理解用户登录状态如何在全局共享
2. 理解 `pinia-plugin-persistedstate` 如何让状态在刷新后不丢失

---

### 任务 2.4：组合式函数 (Composables)（1-2 天）

**学什么**：`use` 前缀的函数、逻辑复用、`provide`/`inject`

**对应项目文件**：
- vue3/src/composables/ — 整个目录都是组合式函数
- 从简单的开始：`utils.ts` → `element.ts` → `messages.ts`
- 实时通信相关：`realtime-messages.ts`、`realtime-images.ts`、`realtime-files.ts`

**关键理解**：组合式函数是这个项目的 **核心架构模式** — 它把复杂逻辑从组件中抽离，使代码可复用、可测试。

---

## 阶段三：数据层 — 前后端如何通信（约 5-7 天）

> **目标**：理解数据如何从 PocketBase 流到 Vue 组件

### 任务 3.1：PocketBase 入门（2 天）

**学什么**：
- PocketBase 是什么：一个单文件的后端服务（数据库 + 认证 + API + 实时订阅）
- 通过 Web UI 操作：创建集合 (collection)、添加字段、CRUD 记录
- PocketBase JS SDK 的基本用法

**实战步骤**：
1. 下载 PocketBase，放到 `pocketbase/` 目录
2. 运行 `pocketbase/start.bat`，访问 `http://127.0.0.1:58090/_/`
3. 在 Web UI 中浏览项目的数据库结构
4. 打开 pocketbase/pb_schema.json — 这是数据库 schema 的 JSON 导出

**对应项目文件**：
- vue3/src/lib/pocketbase/ — PocketBase 客户端初始化
- vue3/src/api/ — 所有 API 调用（消息、文件、图片、用户）

**推荐资源**：[PocketBase 官方文档](https://pocketbase.io/docs/)

---

### 任务 3.2：TanStack Vue Query — 服务端状态管理（2-3 天）

**学什么**：
- `useQuery()` — 获取数据（自动缓存、自动重试、后台刷新）
- `useMutation()` — 修改数据
- `useInfiniteQuery()` — 无限滚动加载
- Query Keys — 缓存键
- `invalidateQueries()` — 手动刷新缓存

**对应项目文件**（这是重点）：
- vue3/src/queries/query-keys.ts — 所有查询键定义
- vue3/src/queries/chat-room-messages.ts — 聊天消息的查询（用了 infinite query 实现双向无限滚动）
- vue3/src/queries/profile.ts — 用户信息查询
- vue3/src/queries/image-page-list.ts — 图片列表查询

**实战练习**：
```ts
// 画出数据流向图：
// PocketBase DB → API 层(api/) → TanStack Query(queries/) → 组件(views/)
```

---

### 任务 3.3：实时通信 — PocketBase Realtime（1-2 天）

**学什么**：PocketBase 的 SSE (Server-Sent Events) 实时订阅

**对应项目文件**：
- vue3/src/api/realtime.ts — 实时订阅的底层 API
- vue3/src/composables/realtime-messages.ts — 消息实时更新逻辑
- vue3/src/stores/realtime-messages.ts — 实时消息状态

**关键理解**：当其他用户发送消息时，PocketBase 通过 SSE 推送事件 → composable 接收 → 更新 TanStack Query 缓存 → UI 自动刷新

---

## 阶段四：UI 组件与样式系统（约 3-5 天）

> **目标**：能修改现有界面、添加新 UI 组件

### 任务 4.1：Element Plus + Naive UI（1-2 天）

**学什么**：
- 项目同时用了两个 UI 库：Element Plus（滚动条、对话框等）和 Naive UI（消息提示、主题等）
- `unplugin-vue-components` 自动导入组件 — 你不需要手动 `import`
- 项目通过 `unplugin-auto-import` 自动导入了 Vue 的 API 和 Naive UI 的 composables

**对应项目文件**：
- vue3/vite.config.ts — 看 `AutoImport` 和 `Components` 插件配置
- vue3/auto-imports.d.ts — 自动导入的类型声明
- vue3/components.d.ts — 自动注册的组件类型声明

### 任务 4.2：项目样式体系（1 天）

**学什么**：TailwindCSS 与 SCSS 的混合使用、暗色模式、CSS 变量

**对应项目文件**：
- vue3/src/assets/styles/main.scss — 样式入口
- vue3/src/assets/styles/color.scss — 颜色变量定义
- vue3/src/assets/styles/element-plus.scss — Element Plus 主题定制

### 任务 4.3：Remixicon 图标（半天）

**学什么**：`<RiSendPlaneFill />` 这类图标组件的使用

**对应项目文件**：vite.config.ts 配置了自动导入，凡 `Ri` 开头的组件自动从 `@remixicon/vue` 导入。

**推荐资源**：[Remixicon 图标浏览](https://remixicon.com/)

---

## 阶段五：跑通完整功能开发流程（约 3-5 天）

> **目标**：模拟一次完整的功能开发，走通从后端到前端的全流程

### 任务 5.1：环境搭建 + 跑通项目（1 天）

```
1. cd vue3 && pnpm install
2. 下载 pocketbase.exe 到 pocketbase/ 目录
3. 运行 pocketbase/start.bat 启动后端
4. 运行 pnpm dev 启动前端
5. 在浏览器中注册账号、发送消息、上传图片 — 作为用户全面体验一遍
```

### 任务 5.2：阅读一个完整功能的代码（1-2 天）

**推荐从"图片浏览"功能入手**（代码量适中、逻辑完整）：

按以下顺序阅读，画出数据流图：

```
1. 数据库 Schema     → pocketbase/pb_schema.json（找 images 相关集合）
2. 生成的 TS 类型    → vue3/src/lib/pocketbase/pocketbase-types.ts
3. API 层            → vue3/src/api/images/
4. Query 层          → vue3/src/queries/image-page-list.ts
                     → vue3/src/queries/image-info.ts
5. 组合式函数        → vue3/src/composables/realtime-images.ts
6. Store             → vue3/src/stores/realtime-images.ts
7. 页面组件          → vue3/src/views/image/
8. 子组件            → vue3/src/components/image/
```

### 任务 5.3：尝试改动一个小功能（1-2 天）

**建议任务**：大致按以下流程开发你的功能
1. 在 PocketBase Web UI 中修改/新增一个集合字段
2. 导出 schema → 更新 pb_schema.json
3. 运行 `pnpm pb-typegen-json` 重新生成 TS 类型
4. 在 `api/` 中编写新的 API 调用
5. 在 `queries/` 中创建新的 Query
6. 在组件中使用 Query 展示数据

---

## 阶段六：进阶专题（按需学习）

| 专题 | 对应项目部分 | 何时学 |
|---|---|---|
| **PocketBase pb_hooks** | pocketbase/pb_hooks/ | 需要写自定义后端逻辑时 |
| **Zod 校验** | 项目依赖中 | 需要表单验证/数据校验时 |
| **PWA** | vite.config.ts 中 `VitePWA` 配置 | 需要改离线功能时 |
| **i18n 国际化** | vue3/src/config/i18n/ + vue3/src/stores/i18n.ts | 需要多语言支持时 |
| **图片处理** | `pica`（压缩）+ `vue-advanced-cropper`（裁剪） | 需要改头像/图片上传功能时 |
| **Docker 部署** | Dockerfile + Dockerfile.release | 需要部署时 |

---

## 学习路线图（时间轴）

```
Week 1  ████████████████████████████████  阶段一：HTML + CSS + JS + TS 基础
Week 2  ████████████████████████████████  阶段二：Vue 3 核心（模板/路由/Pinia/Composables）
Week 3  ████████████████████████████████  阶段三：数据层（PocketBase/Query/Realtime）
Week 4  ████████████████                  阶段四：UI 组件与样式
Week 4  ████████████████████████████████  阶段五：跑通完整开发流程
Week 5+ ████████████████████████████████  阶段六：按需深入 + 独立开发功能
```

---

## 核心原则

1. **不要试图先学完再动手** — 每个任务都伴随实战练习
2. **项目就是最好的教材** — 每学一个概念，立刻去项目文件中找对应代码
3. **阶段一二要快速过** — 不追求精通，目标是「能看懂 .vue 文件」
4. **阶段三是重中之重** — PocketBase + TanStack Query 是本项目最核心的架构
5. **遇到不懂的先跳过** — 标记下来，随着理解加深自然会懂 

Completed: *制定阶段性学习计划* (3/3)

以上计划完全基于 PocketChat 项目的实际技术栈和目录结构制定。每个阶段都标注了**对应的项目文件路径**，方便你学完概念后立刻对照真实代码理解。按照这个路线，大约 4-5 周可以开始独立开发功能。

Similar code found with 1 license type