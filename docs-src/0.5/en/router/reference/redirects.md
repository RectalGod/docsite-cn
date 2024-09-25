# 重定向

在某些情况下，我们可能希望在用户打开特定路径时将他们重定向到另一个页面。我们可以使用 `#[redirect]` 属性告诉路由器这样做。

`#[redirect]` 属性接受一个路由和一个闭包，其中包含路由中定义的所有参数。闭包必须返回一个 [`NavigationTarget`]。

在以下示例中，我们将分别从 `/myblog` 和 `/myblog/:id` 重定向所有人到 `/blog` 和 `/blog/:id`

```rust
{{#include src/doc_examples/full_example.rs:router}}
```

[`NavigationTarget`]: https://docs.rs/dioxus-router/latest/dioxus_router/navigation/enum.NavigationTarget.html