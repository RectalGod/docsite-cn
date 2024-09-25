# 组件

就像你不会想在一个单独的、很长的 `main` 函数中编写一个复杂的程序一样，你也不应该在一个单独的 `App` 函数中构建一个复杂的 UI。相反，你应该将应用程序的功能分解成称为组件的逻辑部分。

组件是一个 Rust 函数，以 UpperCammelCase 命名，它要么不接受参数，要么接受一个属性结构体，并返回一个 `Element` 来描述它想要渲染的 UI。

```rust, no_run
{{#include src/doc_examples/hello_world_desktop.rs:component}}
[[[CODE_BLOCK_aa4beb2c-e1a1-43cd-9b99-6abd5fbdfe9a]]]rust, no_run
{{#include src/doc_examples/components.rs:About}}
[[[CODE_BLOCK_e58e5d4d-0376-42d3-b59a-e0750f096a02]]]inject-dioxus
DemoFrame {
	components::About {}
}
[[[CODE_BLOCK_4d184f05-df08-4129-92dd-ec255f98cd27]]]rust, no_run
{{#include src/doc_examples/components.rs:App}}
[[[CODE_BLOCK_0bddb3e4-6493-4276-9133-d8994a97ca5a]]]inject-dioxus
DemoFrame {
	components::App {}
}
```

> 在这一点上，组件似乎只不过是函数而已。然而，当你更多地了解 Dioxus 的功能时，你会发现它们实际上更加强大！