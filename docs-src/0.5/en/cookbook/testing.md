# 测试

在使用 Dioxus 构建应用程序或库时，您可能希望包含一些测试来检查应用程序部分的行为。本指南将教您如何测试 Dioxus 应用程序的不同部分。

## 组件测试

您可以结合使用 [pretty-assertions](https://docs.rs/pretty_assertions/latest/pretty_assertions/) 和 [dioxus-ssr]() 来检查两个 rsx 片段是否相等：

```rust
{{#include src/doc_examples/component_test.rs}}
```

## Hook 测试

在围绕 Dioxus 创建库时，为您的 [custom hooks](./state/custom_hooks/index.md) 进行测试可能会有所帮助。

Dioxus 目前还没有完整的 Hook 测试库，但您可以通过手动驱动虚拟 DOM 来构建一个定制的测试框架。

```rust
{{#include src/doc_examples/hook_test.rs}}
```

## 端到端测试

您可以使用 [Playwright](https://playwright.dev/) 为您的 Dioxus 应用程序创建端到端测试。

在您的 `playwright.config.js` 中，您需要运行 `cargo run` 或 `dx serve` 而不是默认的构建命令。以下是一段来自端到端 Web 示例的代码片段：

```js
//...
webServer: [
    {
        cwd: path.join(process.cwd(), 'playwright-tests', 'web'),
        command: 'dx serve',
        port: 8080,
        timeout: 10 * 60 * 1000,
        reuseExistingServer: !process.env.CI,
        stdout: "pipe",
    },
],
```

- [Web example](https://github.com/DioxusLabs/dioxus/tree/v0.5/playwright-tests/web)
- [Liveview example](https://github.com/DioxusLabs/dioxus/tree/v0.5/playwright-tests/liveview)
- [Fullstack example](https://github.com/DioxusLabs/dioxus/tree/v0.5/playwright-tests/fullstack)