# 历史按钮

一些平台，比如网页浏览器，为用户提供了一种轻松浏览应用程序历史记录的方法。它们具有 UI 元素或与操作系统集成。

但是，原生平台通常不提供这样的便利设施，这意味着希望用户能够访问历史记录的应用程序需要自行实现。出于这个原因，路由器附带了两个组件，用于模拟浏览器的后退和前进按钮：

- [`GoBackButton`]
- [`GoForwardButton`]

> 如果你想以编程方式浏览历史记录，请查看 [`programmatic navigation`](./navigation/programmatic.md)。

```rust
{{#include src/doc_examples/history_buttons.rs:history_buttons}}
```

如你所知，浏览器通常在没有历史记录可浏览时禁用后退和前进按钮。路由器的历史按钮也试图这样做，但根据 [历史提供者] 情况，这可能无法实现。

重要的是，[`WebHistory`] 均不支持该功能。这是由于浏览器历史 API 的限制。

但是，在这两种情况下，如果没有任何历史记录可供浏览，路由器将忽略按钮按下操作。

此外，当使用 [`WebHistory`] 时，历史按钮可能会将用户导航到应用程序之外的历史记录条目。

[`GoBackButton`]: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.GoBackButton.html
[`GoForwardButton`]: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.GoForwardButton.html