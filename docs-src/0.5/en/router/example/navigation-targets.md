# 导航目标

在上一章中，我们学习了如何在应用内创建指向页面的链接。
我们使用 `target` 属性告诉它们去哪里。此属性接受可以转换为 [`NavigationTarget`] 的内容。

## 什么是导航目标？

[`NavigationTarget`] 类似于 HTML 锚元素的 `href`。它
告诉路由器导航到哪里。Dioxus 路由器知道两种类型的
导航目标：

- [`Internal`]: 我们在上一章中使用过内部链接。它是指向应用内页面的链接，表示为 Route 枚举。
- [`External`]: 这与 HTML 锚点的 `href` 类似。不要将它用于应用内
  导航，因为它会触发浏览器重新加载页面。

## 外部导航

如果我们需要链接到外部页面，我们可以像这样操作：

```rust
{{#include src/doc_examples/external_link.rs:component}}
```

[`External`]: https://docs.rs/dioxus-router/latest/dioxus_router/navigation/enum.NavigationTarget.html#variant.External
[`Internal`]: https://docs.rs/dioxus-router/latest/dioxus_router/navigation/enum.NavigationTarget.html#variant.Internal
[`NavigationTarget`]: https://docs.rs/dioxus-router/latest/dioxus_router/navigation/enum.NavigationTarget.html