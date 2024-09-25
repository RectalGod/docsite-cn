# 创建第一个路由

在本节中，我们将开始使用 Dioxus Router 并为我们的项目添加一个主页和一个 404 页面。

## 基础

Dioxus Router 的核心是 [`Routable`] 宏和 [`Router`] 组件。

Routable 是一个用于任何能够执行以下操作的特性：

- 从 URL 解析
- 转换为 URL
- 渲染为 Element

让我们创建一个新的路由器。首先，我们需要一个实际的页面来进行路由！让我们添加一个主页组件：

```rust
{{#include src/doc_examples/first_route.rs:home}}
```

## 创建路由

我们希望使用 Dioxus Router 将我们的应用程序划分为不同的“页面”。Dioxus Router 将根据 URL 路径确定要渲染哪个页面。

要开始使用 Dioxus Router，我们需要使用 [`Routable`] 宏。

[`Routable`] 宏接受一个枚举，其中包含我们应用程序中所有可能的路由。枚举的每个变体都代表一个路由，并且必须使用 `#[route(path)]` 属性进行标注。

```rust
{{#include src/doc_examples/first_route.rs:router}}
```

[`Router`] 组件将为所有内部组件和钩子提供路由上下文。您通常希望将其放在组件树的顶部。

```rust
{{#include src/doc_examples/first_route.rs:app}}
```

如果您转到应用程序的浏览器选项卡，现在应该在根 URL (`http://localhost:8080/`) 上看到文本 `Welcome to Dioxus Blog!`。如果您为 URL 输入不同的路径，则不应该显示任何内容。

这是因为我们告诉 Dioxus Router 仅当 URL 路径为 `/` 时才渲染 `Home` 组件。

## 备用路由

在我们的示例中，当路由不存在时，Dioxus Router 不会渲染任何内容。许多网站在路径不存在时也会显示“404”页面。让我们在我们的网站上添加一个。

首先，我们创建一个新的 `PageNotFound` 组件。

```rust
{{#include src/doc_examples/catch_all.rs:fallback}}
```

接下来，在 Route 枚举中注册路由以匹配所有其他路由失败的情况。

```rust
{{#include src/doc_examples/catch_all.rs:router}}
```

现在，当您转到不存在的路由时，您应该看到页面未找到文本。

## 结论

在本节中，我们学习了如何创建路由并告诉 Dioxus Router 在 URL 路径为 `/` 时渲染哪个组件。我们还创建了一个 404 页面来处理路由不存在的情况。接下来，我们将创建我们网站的博客部分。我们将使用嵌套路由和 URL 参数。

[`Router`]: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Router.html
[`Routable`]: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Routable.html