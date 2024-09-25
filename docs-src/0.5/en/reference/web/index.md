# 网页

为了在网页上运行，您的应用程序必须编译为 WebAssembly，并依赖于 `dioxus` 和 `dioxus-web` 这些包。

针对网页的 Dioxus 版本构建后，其大小与 React 构建大致相当（70kb 对 65kb），但加载速度更快，因为 [WebAssembly can be compiled as it is streamed](https://hacks.mozilla.org/2018/01/making-webassembly-even-faster-firefoxs-new-streaming-and-tiering-compiler/).

示例：

- [TodoMVC](https://github.com/DioxusLabs/dioxus/blob/main/examples/todomvc.rs)
- [Tailwind App](https://github.com/DioxusLabs/dioxus/tree/v0.5/examples/tailwind)

[![TodoMVC example](https://github.com/DioxusLabs/example-projects/raw/master/todomvc/example.png)](https://github.com/DioxusLabs/dioxus/blob/main/examples/todomvc.rs)

> 注意：由于 Wasm 的限制，[not every crate will work](https://rustwasm.github.io/docs/book/reference/which-crates-work-with-wasm.html) 与您的网页应用程序一起使用，因此您需要确保您的包在没有原生系统调用（计时器、IO 等）的情况下也能正常工作。

## 支持

网页是 Dioxus 支持最好的目标平台。

- 由于您的应用程序将被编译为 WASM，您可以通过 [wasm-bindgen](https://rustwasm.github.io/docs/wasm-bindgen/introduction.html) 访问浏览器 API。
- Dioxus 提供水合功能，可恢复在服务器上渲染的应用程序。有关更多信息，请参阅 [fullstack](../fullstack/index.md) 参考。

## 运行 Javascript

Dioxus 提供了一些对浏览器 API 的人性化封装，但在某些情况下，您可能需要访问 Dioxus 未公开的浏览器 API 部分。

针对这些情况，Dioxus 网页公开 `use_eval` 钩子，允许您在网页视图中运行原始 Javascript：

```rust
{{#include src/doc_examples/eval.rs}}
```

如果您针对网页，但没有计划针对任何其他 Dioxus 渲染器，您也可以使用 [web-sys](https://rustwasm.github.io/wasm-bindgen/web-sys/index.html) 和 [gloo](https://gloo-rs.web.app/) 包中生成的封装。

## 自定义索引模板

Dioxus 支持提供自定义 index.html 模板。index.html 必须包含一个 id 为 `main` 的 `div`，以便使用。热重载仍然受支持。一个示例在 [PWA-Example](https://github.com/DioxusLabs/dioxus/blob/main/examples/PWA-example/index.html) 中提供。