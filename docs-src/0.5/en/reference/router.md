# 路由

在许多应用程序中，你可能需要不同的“场景”。对于网页来说，这些场景可能是包含不同内容的不同网页。对于桌面应用程序来说，这些场景可能是应用程序中的不同视图。

为了统一这些平台，Dioxus 提供了一个名为 Dioxus Router 的内置场景管理解决方案。


## 它是什么？

对于像 Dioxus 着陆页（https://dioxuslabs.com）这样的应用程序，我们希望有几个不同的场景：

- 首页
- 博客

每个场景都是独立的——我们不希望同时渲染首页和博客。

Dioxus 路由器使创建这些场景变得容易。为了确保我们正在使用路由器，请将 `router` 特性添加到你的 `dioxus` 依赖项中：

```shell
cargo add dioxus@0.5.0 --features router
```


## 使用路由器

与 Rust 生态系统中的其他路由器不同，我们的路由器是在编译时声明式构建的。这使得通过定义一个枚举来组合应用程序布局成为可能。

```rust
{{#include src/doc_examples/router_reference.rs:router_definition}}
```

无论何时访问此应用程序，我们都会根据我们输入的路由渲染 Home 组件或 Blog 组件。如果这些路由都不匹配当前位置，则不会渲染任何内容。

我们可以通过两种方式之一解决这个问题：

- 一个回退的 404 页面

```rust
{{#include src/doc_examples/router_reference.rs:router_definition_404}}
```

- 将 404 重定向到首页

```rust
{{#include src/doc_examples/router_reference.rs:router_404_redirect}}
```

## 链接

为了让我们的应用程序导航这些路由，我们可以提供名为链接的可点击元素。这些元素只是简单地包装了 `<a>` 元素，当点击这些元素时，会将应用程序导航到给定位置。因为我们的路由是有效路由的枚举，所以如果你尝试链接到不存在的页面，你将收到编译器错误。

```rust
{{#include src/doc_examples/router_reference.rs:links}}
```

## 更多阅读

此页面只是对路由器的简要概述。有关更多信息，请查看 [router book](../router/index.md) 或一些 [router examples](https://github.com/DioxusLabs/dioxus/blob/master/examples/router.rs).