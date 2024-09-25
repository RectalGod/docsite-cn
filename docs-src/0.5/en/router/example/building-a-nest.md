# 构建一个巢穴

本章我们将开始构建我们网站的博客部分，这将包括链接、嵌套路由和路由参数。

## 网站导航

我们的网站访问者不会知道我们网站上所有可用的页面和博客，所以我们应该为他们提供一个导航栏。我们的导航栏将是一个链接列表，连接着我们的页面。

我们希望我们的导航栏组件在我们网站上的几个不同页面上渲染。为了避免重复代码，我们可以创建一个组件来包裹所有子路由。这被称为布局组件。为了告诉路由器在哪里渲染子路由，我们使用[`Outlet`](https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Outlet.html) 组件。

让我们创建一个新的 `NavBar` 组件：

```rust
{{#include src/doc_examples/nested_routes.rs:nav}}
```

接下来，让我们将我们的 `NavBar` 组件作为布局添加到我们的 Route 枚举中：

```rust
{{#include src/doc_examples/nested_routes.rs:router}}
```

为了向我们的 `NavBar` 添加链接，我们始终可以使用 HTML 锚元素，但这有两个问题：

1. 它会导致整个页面重新加载
2. 我们可能会意外链接到一个不存在的页面

相反，我们想要使用 Dioxus Router 提供的 [`Link`] 组件。

[`Link`] 类似于一个普通的 `<a>` 标签。它接受一个目标和子节点。

与普通的 `<a>` 标签不同，我们可以将我们的 Route 枚举作为目标传递进去。因为我们在路由上标注了 `#[route(path)]` 属性，[`Link`] 将知道如何生成正确的 URL。如果我们使用 Route 枚举，Rust 编译器将阻止我们链接到不存在的页面。

让我们添加我们的链接：

```rust
{{#include src/doc_examples/links.rs:nav}}
```

> 使用这种方法，[`Link`] 组件仅适用于我们应用程序内的链接。要了解更多关于导航目标的信息，请参阅
> [here](./navigation-targets.md).

现在，您应该在页面顶部附近看到一个链接列表。点击其中一个，您应该能够无缝地在页面之间跳转。

## URL 参数和嵌套路由

许多网站，如 GitHub，会在他们的 URL 中放置参数。例如，`https://github.com/DioxusLabs` 使用域名后面的文本动态地搜索和显示关于组织的信息。

我们希望将我们的博客存储在数据库中，并在需要时加载它们。我们还想让我们的用户能够将指向特定博客文章的链接发送给别人。与其在编译时列出所有博客标题，我们可以创建一个动态路由。

我们可以利用一个搜索页面，在点击时加载博客，但这会让我们的用户难以分享我们的博客。这就是 URL 参数的用武之地。

我们博客的路径将类似于 `/blog/myBlogPage`，`myBlogPage` 是 URL 参数。

首先，让我们创建一个布局组件（类似于导航栏），它包裹着博客内容。这允许我们添加一个标题，告诉用户他们在博客上。

```rust
{{#include src/doc_examples/dynamic_route.rs:blog}}
```

现在，我们将创建另一个索引组件，它将在没有选择博客文章时显示：

```rust
{{#include src/doc_examples/dynamic_route.rs:blog_list}}
```

我们还需要创建一个组件来显示实际的博客文章。这个组件将接受 URL 参数作为 props：

```rust
{{#include src/doc_examples/dynamic_route.rs:blog_post}}
```

最后，让我们告诉我们的路由器这些组件：

```rust
{{#include src/doc_examples/dynamic_route.rs:router}}
```

就这样！如果您访问 `/blog/1`，您应该会看到我们的示例文章。

## 结论

在本章中，我们利用了 Dioxus Router 的 Link 和 Route Parameter 功能来构建我们应用程序的博客部分。在下一章中，我们将介绍导航目标（就像我们传递给链接的那个）是如何工作的。

[`Link`]: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Link.html