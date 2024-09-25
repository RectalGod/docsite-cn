# Hooks 和组件状态

到目前为止，我们的组件就像普通的 Rust 函数一样没有状态。然而，在 UI 组件中，拥有状态功能来构建用户交互通常很有用。例如，你可能想要跟踪用户是否打开了下拉菜单并相应地呈现不同的内容。

Hooks 允许我们在组件中创建状态。Hooks 是你在组件中按固定顺序调用的 Rust 函数，它们为组件添加了额外的功能。

Dioxus 提供了许多内置的 Hooks，但如果这些 Hooks 不符合你的特定用例，你也可以 [create your own hook](../cookbook/state/custom_hooks/index.md)

## use_signal Hook

[`use_signal`](https://docs.rs/dioxus/latest/dioxus/prelude/fn.use_signal.html) 是最简单的 Hooks 之一。

- 你提供一个闭包来确定初始值：`let mut count = use_signal(|| 0);`
- `use_signal` 提供给你当前值，以及一种写入该值的方式
- 当值更新时，`use_signal` 使组件重新渲染（以及任何引用它的其他组件），然后为你提供新值。


例如，你可能已经看到过计数器示例，其中使用 `use_signal` Hook 跟踪状态（一个数字）：

```rust, no_run
{{#include src/doc_examples/hooks_counter.rs:component}}
[[[CODE_BLOCK_13a878ee-262e-4249-b2e9-6d4dc54f7501]]]inject-dioxus
DemoFrame {
   hooks_counter::App {}
}
[[[CODE_BLOCK_f48fdd4e-8fe0-42d8-b27a-3a3754c8e9b5]]]rust, no_run
{{#include src/doc_examples/hooks_counter_two_state.rs:component}}
[[[CODE_BLOCK_e2b8d36f-836d-4a4e-ae34-d26bbf80466c]]]inject-dioxus
DemoFrame {
  hooks_counter_two_state::App {}
}
[[[CODE_BLOCK_15f0f835-b82e-4771-94eb-b1af8a53a2af]]]rust, no_run
{{#include src/doc_examples/hooks_use_signal.rs:component}}
[[[CODE_BLOCK_9d5b8df3-b141-4f51-b9ae-c13255b77318]]]inject-dioxus
DemoFrame {
  hooks_use_signal::App {}
}
[[[CODE_BLOCK_4d94f16c-f05b-4b2e-8801-d192b760312e]]]rust, no_run
{{#include src/doc_examples/hooks_counter_two_state.rs:use_signal_calls}}
[[[CODE_BLOCK_e191901b-5a42-4a07-885d-a686b297530d]]]rust, no_run
{{#include src/doc_examples/hooks_bad.rs:conditional}}
[[[CODE_BLOCK_e02b9b1b-0a2e-4654-a913-0c3027b19562]]]rust, no_run
{{#include src/doc_examples/hooks_bad.rs:closure}}
[[[CODE_BLOCK_c2a18f18-638c-47e5-8c5d-2eb2f9771dbd]]]rust, no_run
{{#include src/doc_examples/hooks_bad.rs:loop}}
```

## 附加资源

- [dioxus_hooks API docs](https://docs.rs/dioxus-hooks/latest/dioxus_hooks/)
- [dioxus_hooks source code](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/hooks)