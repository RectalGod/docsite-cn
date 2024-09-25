# 项目结构

Dioxus 组织中包含许多包。本文档将帮助您了解每个包的目的及其相互关系。

## 渲染器

- [Desktop](https://github.com/DioxusLabs/dioxus/tree/main/packages/desktop):  一个在本地运行 Dioxus 应用程序的渲染器，但使用系统 Webview 进行渲染。
- [Mobile](https://github.com/DioxusLabs/dioxus/tree/main/packages/mobile):  一个在本地运行 Dioxus 应用程序的渲染器，但使用系统 Webview 进行渲染。目前，它是桌面渲染器的副本。
- [Web](https://github.com/DioxusLabs/dioxus/tree/main/packages/Web): 通过编译成 WASM 并操作 DOM，在浏览器中渲染 Dioxus 应用程序。
- [Liveview](https://github.com/DioxusLabs/dioxus/tree/main/packages/liveview):  一个在服务器上运行的渲染器，使用 websocket 代理在浏览器中进行渲染。
- [Plasmo](https://github.com/DioxusLabs/blitz/tree/master/packages/plasmo):  一个将 HTML 类树渲染到终端的渲染器。
- [TUI](https://github.com/DioxusLabs/blitz/tree/master/packages/dioxus-tui): 一个使用 Plasmo 在终端中渲染 Dioxus 应用程序的渲染器。
- [Blitz-Core](https://github.com/DioxusLabs/blitz/tree/master/packages/blitz-core):  一个使用 WGPU 渲染 HTML 类树的实验性原生渲染器。
- [Blitz](https://github.com/DioxusLabs/blitz):  一个使用 Blitz-Core 和 WGPU 渲染 Dioxus 应用程序的实验性原生渲染器。
- [SSR](https://github.com/DioxusLabs/dioxus/tree/main/packages/ssr):  一个在服务器上运行 Dioxus 应用程序并将其渲染为 HTML 的渲染器。

## 状态管理/钩子

- [Hooks](https://github.com/DioxusLabs/dioxus/tree/main/packages/hooks): Dioxus 应用程序中常用钩子的集合。
- [Signals](https://github.com/DioxusLabs/dioxus/tree/main/packages/signals):  Dioxus 应用程序的实验性状态管理库。目前，它包含一个 `Copy` 版本的 Signal。
- [SDK](https://github.com/DioxusLabs/sdk):  一个与系统接口（剪贴板、相机等）进行交互的平台无关钩子集合。
- [Fermi](https://github.com/DioxusLabs/dioxus/tree/main/packages/fermi):  Dioxus 应用程序的全局状态管理库。
- [Router](https://github.com/DioxusLabs/dioxus/tree/main/packages/router):  Dioxus 应用程序的客户端路由器。

## 核心工具

- [core](https://github.com/DioxusLabs/dioxus/tree/main/packages/core): 每个 Dioxus 应用程序使用的核心虚拟 DOM 实现。
  - 您可以在 [in this blog post](https://dioxuslabs.com/blog/templates-diffing/) 中了解有关核心架构的更多信息，以及 [custom renderer section of the guide](../custom_renderer/index.md)。
- [RSX](https://github.com/DioxusLabs/dioxus/tree/main/packages/RSX): 用于热重载、自动格式化和宏的 RSX 核心解析器。
- [core-macro](https://github.com/DioxusLabs/dioxus/tree/main/packages/core-macro): 用于编写 Dioxus 应用程序的 rsx! 宏。（它是 RSX crate 的包装器）
- [HTML macro](https://github.com/DioxusLabs/dioxus-html-macro):  RSX 宏的 HTML 类替代方案。

## 原生渲染器工具

- [native-core](https://github.com/DioxusLabs/blitz/tree/main/packages/native-core): 递增计算的状态树（主要是样式）
  - 您可以在 [custom renderer section of the guide](../custom_renderer/index.html#native-core) 中了解有关原生核心如何帮助您构建原生渲染器的更多信息。
- [native-core-macro](https://github.com/DioxusLabs/blitz/tree/main/packages/native-core-macro):  原生核心的辅助宏。
- [Taffy](https://github.com/DioxusLabs/taffy): 支持 Blitz-Core、Plasmo 和 Bevy UI 的布局引擎。

## Web 渲染器工具

- [HTML](https://github.com/DioxusLabs/dioxus/tree/main/packages/html):  定义了 HTML 特定的元素、事件和属性。
- [Interpreter](https://github.com/DioxusLabs/dioxus/tree/main/packages/interpreter):  定义了 Web 和桌面渲染器使用的浏览器绑定。

## 开发者工具

- [hot-reload](https://github.com/DioxusLabs/dioxus/tree/main/packages/hot-reload):  一个使用 RSX crate 来热重载任何 rsx! 宏的静态部分的宏。此宏适用于具有 [integration](https://crates.io/crates/dioxus-hot-reload) 的任何非 Web 渲染器。
- [autofmt](https://github.com/DioxusLabs/dioxus/tree/main/packages/autofmt):  格式化 RSX 代码。
- [rsx-rosetta](https://github.com/DioxusLabs/dioxus/tree/main/packages/RSX-rosetta):  处理 HTML 和 RSX 之间的转换。
- [CLI](https://github.com/DioxusLabs/dioxus/tree/main/packages/cli):  一个命令行界面和 VSCode 扩展，用于辅助 Dioxus 使用。