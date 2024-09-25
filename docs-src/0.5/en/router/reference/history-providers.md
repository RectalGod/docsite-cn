# 历史提供者

[`HistoryProvider`] 用于路由器跟踪导航历史并更新任何外部状态（例如浏览器的 URL）。

路由器提供两个 [`HistoryProvider`]，但您也可以创建自己的。
两个默认实现是：

- [`MemoryHistory`] 是一个在内存中工作的自定义实现。
- [`LiveviewHistory`] 是一个与 liveview 渲染器一起工作的自定义实现。
- [`WebHistory`] 与浏览器的 URL 集成。

默认情况下，路由器使用 [`MemoryHistory`]。当 `web` 功能处于活动状态时，它可能会更改为使用 [`WebHistory`]，但这并不保证。

您可以覆盖默认历史记录：

```rust
{{#include src/doc_examples/history_provider.rs:app}}
```