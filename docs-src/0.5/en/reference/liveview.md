# Liveview

Liveview 允许应用程序在服务器上*运行*并在浏览器中*渲染*。它使用 WebSockets 在服务器和浏览器之间进行通信。

示例：
- [Simple Example](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/liveview/examples/axum.rs)

## 支持

Dioxus liveview 将在未来版本中迁移到 [dioxus-fullstack](./fullstack/index.md)。迁移完成后，您可能需要更新代码。我们计划让这次迁移尽可能简单。

与 Web 平台相比，Liveview 目前功能有限。Liveview 应用程序在服务器上的原生线程中运行。这意味着浏览器 API 不可使用，因此渲染 WebGL、Canvas 等不像 Web 那样容易。但是，原生系统 API 是可访问的，因此流式传输、WebSockets、文件系统等都是可行的 API。

## 路由器集成

目前，Dioxus 路由器并未与 liveview 渲染器中的浏览器历史记录集成。如果您有兴趣为 Dioxus 贡献此功能，请在 [here](https://github.com/DioxusLabs/dioxus/issues/1038) 跟踪该问题。

## 管理延迟

Liveview 使从客户端与服务器通信变得极其方便，但同时也存在一些缺点。在 Dioxus Liveview 中，默认情况下，每个交互都会经过服务器。


因此，在使用 liveview 渲染器时，您需要非常小心地管理延迟。在其他渲染器（例如 [controlled inputs](../../reference/user_input.md)）上足够快的事件，在 liveview 渲染器中使用起来可能会很令人沮丧。


为了解决这个问题，您可以在 liveview 应用程序中注入一些 JavaScript 代码。如果使用原始属性作为监听器，您可以注入一些将在事件触发时运行的 JavaScript 代码：

```rust
rsx! {
    div {
        input {
            "oninput": "console.log('input changed!')"
        }
    }
}
```