# 路由更新回调

在某些情况下，我们可能希望在当前路由发生变化时运行自定义代码。为此，[`RouterConfig`] 公开了 `on_update` 字段。

## 回调的行为

`on_update` 在当前路由信息发生变化时被调用。它在路由器更新其内部状态后被调用，但在依赖组件和钩子更新之前被调用。

如果回调返回 [`NavigationTarget`], 路由器将使用指定的目标替换当前位置。它不会再次调用 `on_update`。

如果在任何时候路由器遇到导航失败，它将进入适当的状态，而不会调用 `on_update`。无论无效的目标是否启动了导航，是否作为重定向目标被找到，或者是否由 `on_update` 本身返回，都没有关系。

## 代码示例

```rust
{{#include src/doc_examples/routing_update.rs:router}}
```

[`NavigationTarget`]: https://docs.rs/dioxus-router/latest/dioxus_router/navigation/enum.NavigationTarget.html
[`RouterConfig`]: https://docs.rs/dioxus-router/latest/dioxus_router/prelude/struct.RouterConfig.html