# 将路由添加到你的应用程序

在本章中，我们将学习如何将路由添加到我们的应用程序。它本身并没有什么用处，但是它是其他章节中描述的所有功能的前提条件。

> 确保你添加了 `dioxus-router` 依赖项，如 [introduction](../index.md) 中所述。

在大多数情况下，我们希望将路由添加到应用程序的根组件。这样，我们可以确保我们在任何地方都可以访问它的所有功能。

首先，我们使用 router 宏定义路由：

```rust
{{#include src/doc_examples/first_route.rs:router}}
```

然后，我们使用 [`Router`] 组件渲染路由。

```rust
{{#include src/doc_examples/first_route.rs:app}}
```