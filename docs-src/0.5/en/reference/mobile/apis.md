# 移动端

本指南将涵盖 Dioxus 移动端渲染器的特定概念。

## 运行 JavaScript

Dioxus 提供了一些浏览器 API 的便捷封装，但在某些情况下，您可能需要访问 Dioxus 未公开的浏览器 API 部分。

对于这些情况，Dioxus 桌面端公开了 `use_eval` 钩子，它允许您在 Web 视图中运行原始 JavaScript：

```rust
{{#include src/doc_examples/eval.rs}}
```

## 自定义资源

您可以链接到 dioxus 移动端中的本地资源，而不是使用 URL：

```rust
{{#include src/doc_examples/custom_assets.rs}}
```

## 与 Wry 集成

在您需要对窗口进行更低级控制的情况下，您可以使用通过 [Desktop Config](https://docs.rs/dioxus-desktop/0.5.0/dioxus_desktop/struct.Config.html) 和 [use_window hook](https://docs.rs/dioxus-desktop/0.5.0/dioxus_desktop/struct.DesktopContext.html) 公开的 wry API。