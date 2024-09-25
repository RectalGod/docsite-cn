# Dioxus 参考

这份参考包含了[guide](../guide/index.md)中所有概念的更详细解释以及更多内容。

## 渲染

- [RSX](rsx.md): Rsx 是一种类似 HTML 的宏，允许您声明 UI
- [Components](components.md): 组件是 Dioxus 中 UI 的构建块
- [Props](component_props.md): Props 允许您将信息传递给组件
- [Event Listeners](event_handlers.md): 事件监听器让您能够响应用户输入
- [User Input](user_input.md): 如何在 Dioxus 中处理用户输入
- [Dynamic Rendering](dynamic_rendering.md): 如何在 Dioxus 中动态渲染数据

## 状态

- [Hooks](hooks.md): Hooks 允许您创建组件状态
- [Context](context.md): Context 允许您在父级创建状态，并在子级中使用它
- [Routing](router.md): 路由器帮助您管理 URL 状态
- [Resource](use_resource.md): Use future 允许您创建一个异步任务并监控它的状态
- [UseCoroutine](use_coroutine.md): Use coroutine 帮助您管理外部状态
- [Spawn](spawn.md): Spawn 创建一个异步任务

## 平台

- [Choosing a Web Renderer](choosing_a_web_renderer.md): 不同 Web 渲染器的概述
- [Desktop](desktop/index.md): 桌面特定 API 的概述
- [Web](web/index.md): Web 特定 API 的概述
- [Fullstack](fullstack/index.md): 全栈特定 API 的概述
    - [Server Functions](fullstack/server_functions.md): 服务器函数使在您的服务器和客户端之间进行通信变得容易
    - [Extractors](fullstack/extractors.md): 提取器允许您从请求的标头中获取额外的信息
    - [Middleware](fullstack/middleware.md): 中间件允许您包装服务器函数请求或响应
    - [Authentication](fullstack/authentication.md): 如何使用服务器函数处理身份验证的概述
    - [Routing](fullstack/routing.md): 如何在全栈渲染器中使用路由器的概述
- [SSR](ssr.md): SSR 渲染器的概述
- [TUI](tui.md): Web 特定 API 的概述
- [Liveview](liveview.md): Liveview 特定 API 的概述