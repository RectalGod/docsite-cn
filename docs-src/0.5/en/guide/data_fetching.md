# 获取数据

本章我们将从 Hacker News API 获取数据并使用它来呈现应用程序中的热门帖子列表。

## 定义 API

首先，我们需要创建一些工具来使用 [reqwest](https://docs.rs/reqwest/latest/reqwest/index.html) 从 Hacker News API 获取数据：

```rust
{{#include src/doc_examples/hackernews_async.rs:api}}
```

上面的代码要求你添加 [reqwest](https://crates.io/crates/reqwest), [async_recursion](https://crates.io/crates/async-recursion), 和 [futures](https://crates.io/crates/futures) 依赖：

```bash
cargo add reqwest --features json
cargo add async_recursion
cargo add futures
```

对这些依赖的简要概述：
- [reqwest](https://crates.io/crates/reqwest) 允许我们创建到 Hacker News API 的 HTTP 请求。
- [async_recursion](https://crates.io/crates/async-recursion) 提供了一个实用宏，允许我们递归地使用异步函数。
- [futures](https://crates.io/crates/futures) 为我们提供了围绕 Rust 的 Futures 的实用工具。


## 使用异步

[`use_resource`](https://docs.rs/dioxus-hooks/latest/dioxus_hooks/fn.use_resource.html) 是一个 [hook](./state.md)，它允许你运行一个异步闭包，并提供给你它的结果。

例如，我们可以在 `use_resource` 中进行 API 请求（使用 [reqwest](https://docs.rs/reqwest/latest/reqwest/index.html))：

```rust
{{#include src/doc_examples/hackernews_async.rs:use_resource}}
```

`use_resource` 中的代码将在组件渲染后提交给 Dioxus 调度程序。

我们可以使用 `.read()` 来获取未来的结果。在第一次运行时，由于组件加载时没有准备好的数据，它的值将为 `None`。但是，一旦未来完成，组件将被重新渲染，并且值现在将为 `Some(...)`，包含闭包的返回值。

然后，我们可以通过遍历每个帖子并使用 `StoryListing` 组件渲染它们来渲染结果。

```inject-dioxus
DemoFrame {
	hackernews_async::fetch::App {}
}
```

> 你可以在 [Async reference](../reference/index.md) 中阅读更多关于在 Dioxus 中使用异步的信息。

## 延迟获取数据

最后，当用户将鼠标悬停在帖子上的时候，我们将延迟获取每个帖子的评论。


我们需要重新审视处理将鼠标悬停在项目上的代码。我们可以不传递空评论列表，而是在用户将鼠标悬停在项目上时获取所有相关的评论。


我们将使用 [use_signal](https://docs.rs/dioxus-hooks/latest/dioxus_hooks/fn.use_signal.html) 钩子缓存评论列表。此钩子允许你在单个组件中存储一些状态。当用户触发获取评论时，我们将检查响应是否已被缓存，然后再从 Hacker News API 获取数据。

```rust
{{#include src/doc_examples/hackernews_async.rs:resolve_story}}
```

```inject-dioxus
DemoFrame {
	hackernews_async::App {}
}
```