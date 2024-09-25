# 桌面

本指南将涵盖 Dioxus 桌面渲染器特有的概念。

使用 Dioxus 桌面构建的应用程序使用系统 WebView 渲染页面。这使得应用程序的最终大小远小于其他 WebView 渲染器（通常小于 5MB）。

虽然桌面应用程序是在 WebView 中渲染的，但您的 Rust 代码是本地运行的。这意味着浏览器 API _不可用_，因此渲染 WebGL、Canvas 等不像 Web 那样容易。但是，本地系统 API _可用_，因此流、WebSocket、文件系统等都可以通过系统 API 很容易地访问。

Dioxus 桌面是基于 [Tauri](https://tauri.app/) 构建的。目前，对菜单栏、事件处理等方面的 Dioxus 抽象有限。在某些情况下，您可能需要直接利用 Tauri - 通过 [Wry](http://github.com/tauri-apps/wry/) 和 [Tao](http://github.com/tauri-apps/tao)。

> 将来，我们计划迁移到基于自定义 Web 渲染器的 DOM 渲染器，并集成 WGPU ([Blitz](https://github.com/DioxusLabs/blitz))。

## 示例

- [File Explorer](https://github.com/DioxusLabs/dioxus/blob/main/examples/file_explorer.rs)
- [Tailwind App](https://github.com/DioxusLabs/dioxus/tree/v0.5/examples/tailwind)

[![Tailwind App screenshot](./public/static/tailwind_desktop_app.png)](https://github.com/DioxusLabs/dioxus/tree/v0.5/examples/tailwind)

## 运行 Javascript

Dioxus 提供了一些对浏览器 API 的便捷包装器，但在某些情况下，您可能需要访问 Dioxus 未公开的浏览器 API 部分。


对于这些情况，Dioxus 桌面公开了 use_eval hook，它允许您在 webview 中运行原始 Javascript：

```rust
{{#include src/doc_examples/eval.rs}}
```

## 自定义资产

您可以在 dioxus 桌面中链接到本地资产，而不是使用 URL：

```rust
{{#include src/doc_examples/custom_assets.rs}}
```

您可以在 [assets](./assets.md) 参考中详细了解资产。

## 与 Wry 集成

在您需要对窗口进行更低级控制的情况下，您可以使用通过 [Desktop Config](https://docs.rs/dioxus-desktop/0.5.0/dioxus_desktop/struct.Config.html) 和 [use_window hook](https://docs.rs/dioxus-desktop/0.5.0/dioxus_desktop/fn.use_window.html) 公开的 wry API。