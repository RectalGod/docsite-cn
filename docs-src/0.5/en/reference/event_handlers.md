# 事件处理程序

事件处理程序用于响应用户操作。例如，当用户点击、滚动、移动鼠标或输入字符时，可以触发一个事件处理程序。

事件处理程序附加到元素上。例如，我们通常不关心应用中发生的所有点击，只关心特定按钮上的点击。

事件处理程序类似于常规属性，但它们的名称通常以 `on` 开头，并接受闭包作为值。闭包将在监听的事件触发时被调用，并传递该事件。

例如，要处理元素上的点击，我们可以指定一个 `onclick` 处理程序：

```rust, no_run
{{#include src/doc_examples/event_click.rs:rsx}}
[[[CODE_BLOCK_1de46db1-66a3-405c-b966-864499f96a46]]]inject-dioxus
DemoFrame {
    event_click::App {}
}
[[[CODE_BLOCK_c0f2db9e-f6db-4d8e-9e4c-2116a78e4ef7]]]
Clicked! Event: UiEvent { bubble_state: Cell { value: true }, data: MouseData { coordinates: Coordinates { screen: (242.0, 256.0), client: (26.0, 17.0), element: (16.0, 7.0), page: (26.0, 17.0) }, modifiers: (empty), held_buttons: EnumSet(), trigger_button: Some(Primary) } }
Clicked! Event: UiEvent { bubble_state: Cell { value: true }, data: MouseData { coordinates: Coordinates { screen: (242.0, 256.0), client: (26.0, 17.0), element: (16.0, 7.0), page: (26.0, 17.0) }, modifiers: (empty), held_buttons: EnumSet(), trigger_button: Some(Primary) } }
[[[CODE_BLOCK_782512b7-728f-40cb-93d8-55c89bbde5f5]]]rust, no_run
{{#include src/doc_examples/event_nested.rs:rsx}}
[[[CODE_BLOCK_2ce4800d-2785-4043-b399-1af16f134f9c]]]rust, no_run
{{#include src/doc_examples/event_prevent_default.rs:prevent_default}}
[[[CODE_BLOCK_ed26c8c6-2607-4a0d-a895-af97880d4310]]]inject-dioxus
DemoFrame {
    event_prevent_default::App {}
}
[[[CODE_BLOCK_8d4f2bd9-09c0-491c-8624-e26ab437a546]]]rust, no_run
{{#include src/doc_examples/event_handler_prop.rs:component_with_handler}}
[[[CODE_BLOCK_3233b273-b06c-4c4e-af0b-e715b8c716e7]]]rust, no_run
{{#include src/doc_examples/event_handler_prop.rs:usage}}
[[[CODE_BLOCK_b6ead1a2-335a-4160-86a0-b4cfe576f2d0]]]rust, no_run
{{#include src/doc_examples/event_handler_prop.rs:async}}
[[[CODE_BLOCK_e5fc5939-6f52-4883-8716-e83cbe2c6484]]]rust, no_run
{{#include src/doc_examples/event_handler_prop.rs:custom_data}}
```