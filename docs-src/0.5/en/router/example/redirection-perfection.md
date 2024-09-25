# 重定向精通

您已经踏上了成为路由大师的道路！

本章我们将介绍如何创建重定向。

## 创建重定向

重定向非常简单。当 Dioxus 在寻找要渲染的组件时遇到重定向时，它会将用户重定向到重定向的目标。

举个简单的例子，假设您希望用户即使使用 `/myblog` 或 `/myblog/:name` 的路径，也能访问您的博客。

重定向是路由枚举中的特殊属性，它接受一个路由和一个带有路由参数的闭包。闭包应该返回一个要重定向到的路由。

让我们在我们的路由枚举中添加一个重定向：

```rust
{{#include src/doc_examples/full_example.rs:router}}
```

就是这样！现在您的用户将被重定向到博客。

### 结论

做得很好！您已经完成了 Dioxus Router 指南。您已经构建了一个小型应用程序，并了解了使用 Dioxus Router 可以做很多事情。
要继续您的旅程，您可以尝试以下列出的挑战，查看 [router examples](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/router/examples) 或 [API reference](https://docs.rs/dioxus-router/)。

### 挑战

- 将您的组件组织到单独的文件中，以提高可维护性。
- 如果您还没有，请为您的应用程序添加一些样式。
- 构建一个关于页面，让您的访问者了解您是谁。
- 添加一个使用 URL 参数的用户系统。
- 创建一个简单的管理系统来创建、删除和编辑博客。
- 如果您想挑战极限，请将您的应用程序连接到 REST API 和数据库。