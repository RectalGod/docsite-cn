# 布局

布局允许你将所有子路由包装在一个组件中。这在创建诸如将在许多不同路由中使用的标题之类的组件时非常有用。

[`Outlet`] 告诉路由器在布局中渲染内容的位置。在以下示例中，
Index 将在 [`Outlet`] 中渲染。

此页面使用 Dioxus 构建。它在多个不同位置使用布局。以下是当前页面上布局使用方式的概述。将鼠标悬停在不同的布局上以查看它们在页面上的元素。

```inject-dioxus
LayoutsExplanation {}
```

这是一个更完整的布局包装页面主体的示例。

```rust
{{#include src/doc_examples/outlet.rs:outlet}}
```

上面的示例将输出以下 HTML（为了可读性添加了换行符）：

```html
<header>header</header>
<h1>Index</h1>
<footer>footer</footer>
```

## 带有动态段的布局

你可以将布局与 [nested routes](./routes/nested.md) 结合使用以创建动态布局，其内容根据当前路由而改变。

与路由一样，布局组件必须为路由中的每个动态段接受一个 prop。例如，如果你有一个具有动态段的路由，例如 `/:name`, 你的布局组件必须接受一个 `name` prop：

```rust
{{#include src/doc_examples/outlet.rs:outlet_with_params}}
```

或者，要获取完整路由，可以使用 `use_route` hook。

```rust
{{#include src/doc_examples/outlet.rs:outlet_route}}
```

[`Outlet`]: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Outlet.html
[`use_route`]: https://docs.rs/dioxus-router/latest/dioxus_router/hooks/fn.use_route.html