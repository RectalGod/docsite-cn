# 服务器端渲染

为了对渲染过程进行更底层的控制，你可以直接使用 `dioxus-ssr` 板条箱。当与不支持 `dioxus-fullstack` 的 Web 框架集成或预渲染页面时，这将非常有用。

## 设置

在本指南中，我们将展示如何使用 Dioxus SSR 与 [Axum](https://docs.rs/axum/latest/axum/) 一起使用。

确保你已安装 Rust 和 Cargo，然后创建一个新项目：

```shell
cargo new --bin demo
cd demo
```

将 Dioxus 和 ssr 渲染器添加为依赖项：

```shell
cargo add dioxus@0.5.0
cargo add dioxus-ssr@0.5.0
```

接下来，添加所有 Axum 依赖项。如果你使用的是不同的 Web 框架，这将有所不同。

```
cargo add tokio --features full
cargo add axum
```

你的依赖项应该大致如下所示：

```toml
[dependencies]
axum = "0.7"
dioxus = { version = "*" }
dioxus-ssr = { version = "*" }
tokio = { version = "1.15.0", features = ["full"] }
```

现在，设置你的 Axum 应用程序以在端点上响应。

```rust
{{#include src/doc_examples/ssr.rs:main}}
```

然后添加我们的端点。我们可以直接渲染 `rsx!`：

```rust
{{#include src/doc_examples/ssr.rs:app_endpoint}}
```

或者我们可以渲染 VirtualDoms。

```rust
{{#include src/doc_examples/ssr.rs:app_endpoint_vdom}}
```

最后，你可以使用 `cargo run` 而不是 `dx serve` 来运行它。

## 多线程支持

Dioxus VirtualDom 目前不是 `Send`。在内部，我们使用相当多的内部可变性，这对于线程是不安全的。
当使用需要 `Send` 的 Web 框架时，可以将 VirtualDom 立即渲染到字符串中，但不能在等待点上保存 VirtualDom。对于保留状态的 SSR（本质上是 LiveView），你需要在自己的线程上生成一个 VirtualDom，并通过通道与它通信，或者创建一个 VirtualDom 池。
你可能会注意到你无法在等待点上保存 VirtualDom。因为 Dioxus 目前不是线程安全的，它_必须_保留在它启动的线程上。我们正在努力放松这个要求。