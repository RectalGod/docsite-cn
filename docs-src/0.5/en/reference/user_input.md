# 用户输入

界面通常需要提供一种输入数据的方式，例如文本、数字、复选框等。在 Dioxus 中，您可以通过两种方式处理用户输入。

## 受控输入

使用受控输入，您可以直接控制输入的状态。这为您提供了很大的灵活性，并使保持同步变得容易。例如，以下是创建受控文本输入的方式：

```rust, no_run
{{#include src/doc_examples/input_controlled.rs:component}}
[[[CODE_BLOCK_e85e5b2d-e038-47ec-b669-9662d6be05b9]]]inject-dioxus
DemoFrame {
    input_controlled::App {}
}
[[[CODE_BLOCK_ee802f5c-ab76-4900-b975-6b3f024b1891]]]rust, no_run
{{#include src/doc_examples/input_uncontrolled.rs:component}}
[[[CODE_BLOCK_94893ed0-dd7a-4c85-9e9e-cc3f35113c2c]]]inject-dioxus
DemoFrame {
    input_uncontrolled::App {}
}
[[[CODE_BLOCK_c3631969-e625-4104-b2a5-9788ed04106e]]]
Submitted! UiEvent { data: FormData { value: "", values: {"age": "very old", "date": "1966", "name": "Fred"} } }
[[[CODE_BLOCK_56fca554-afb9-4873-81f1-6cda0ae1a3e9]]]rust, no_run
{{#include src/doc_examples/input_fileengine.rs:component}}
[[[CODE_BLOCK_770ff30a-20b2-42f6-a6d9-a17ed25f5484]]]rust, no_run
{{#include src/doc_examples/input_fileengine_async.rs:onchange_event}}
[[[CODE_BLOCK_5adacf57-209d-4f2e-9807-ac5197ff9091]]]rust, no_run
{{#include src/doc_examples/input_fileengine_folder.rs:rsx}}
```