# 简介

Dioxus 是一个用于构建跨平台用户界面的可移植、高效、符合人体工程学的 Rust 框架。本指南将帮助您开始编写用于 Web、桌面、移动设备等的 Dioxus 应用。

```rust
{{#include src/doc_examples/readme.rs}}
```

```inject-dioxus
DemoFrame {
    readme::App {}
}
```

Dioxus 很大程度上受到了 React 的启发。如果您了解 React，那么开始使用 Dioxus 将非常容易。

> 本指南假设您已经了解一些 [Rust](https://www.rust-lang.org/)！如果您不了解，我们建议您先阅读 [*the book*](https://doc.rust-lang.org/book/ch01-00-getting-started.html) 来学习 Rust。

## 特性

- 使用三行代码创建跨平台应用程序。（Web、桌面、服务器、移动设备等）
- 非常符合人体工程学且功能强大的状态管理，结合了 react、solid 和 svelte 的最佳部分。
- 完整的内联文档 - 所有 HTML 元素、监听器和事件的悬停和指南。
- 高性能应用程序 [approaching the fastest web frameworks on the web](https://dioxuslabs.com/blog/templates-diffing) 和桌面上的原生速度。
- 一流的异步支持。

### 多平台

Dioxus 是一个 *可移植* 的工具包，这意味着核心实现可以在任何地方运行，无需依赖平台的链接。与许多其他 Rust 前端工具包不同，Dioxus 本身并不与 WebSys 相关联。事实上，每个元素和事件监听器都可以在编译时进行替换。默认情况下，Dioxus 附带启用 `html` 功能，但可以根据您的目标渲染器禁用它。

目前，我们有几个第一方渲染器：
- WebSys/Sledgehammer（用于 WASM）：支持良好
- Tao/Tokio（用于桌面应用程序）：支持良好
- Tao/Tokio（用于移动应用程序）：支持较差
- 全栈（用于 SSR 和服务器函数）：支持良好
- TUI/Plasmo（用于基于终端的应用程序）：实验性

## 稳定性

Dioxus 尚未发布稳定版本。

Web：由于 Web 是一个相当成熟的平台，我们预计 Web 功能的 API 变化将非常少。

桌面：随着我们找到比 ElectronJS 对手更好的模式，API 可能会有所变化。

全栈：随着我们找到服务器通信的最佳 API，API 可能会有所变化。