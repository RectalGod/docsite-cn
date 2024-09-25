# 描述 UI

Dioxus 是一个 _声明式_ 框架。这意味着我们不是告诉 Dioxus 要做什么（例如，"创建一个元素" 或 "将颜色设置为红色"），而是简单地 _声明_ 我们希望 UI 呈现的样子，使用 RSX。

你已经在 "hello world" 应用中看到了 RSX 语法的简单示例：

```rust, no_run
{{#include src/doc_examples/hello_world_desktop.rs:component}}
[[[CODE_BLOCK_4ae20c6b-5e64-463f-a8fc-a93e7ce99cd6]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:button}}
[[[CODE_BLOCK_83e07c5c-0383-4105-9bc2-2c5c8c4e6123]]]inject-dioxus
DemoFrame {
	rsx_overview::Button {}
}
[[[CODE_BLOCK_3b987ccc-0cb4-4723-a459-d340570db6d2]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:attributes}}
[[[CODE_BLOCK_a6768e1b-2b9c-46ac-b215-4ae0939340ea]]]inject-dioxus
DemoFrame {
	rsx_overview::Attributes {}
}
[[[CODE_BLOCK_fecc3ad7-8143-40b7-a769-1517a3fec81f]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:attributes_type}}
[[[CODE_BLOCK_89e4d2fd-6f58-468f-8177-44ab499d6b99]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:conditional_attributes}}
[[[CODE_BLOCK_65c41cd4-1d4d-432a-b464-1078cd6ec54c]]]inject-dioxus
DemoFrame {
	rsx_overview::ConditionalAttributes {}
}
[[[CODE_BLOCK_e4efcb28-3394-4be3-b76e-3df0c564d47a]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:custom_attributes}}
[[[CODE_BLOCK_a068429d-0fc5-4ba6-b137-0596136a37e0]]]inject-dioxus
DemoFrame {
	rsx_overview::CustomAttributes {}
}
[[[CODE_BLOCK_58e86d56-1378-42a9-8e52-3222ed746060]]]rust, no_run
{{#include src/doc_examples/dangerous_inner_html.rs:dangerous_inner_html}}
[[[CODE_BLOCK_ff74921e-2ef7-4835-be49-dc0bb8b96651]]]inject-dioxus
DemoFrame {
	dangerous_inner_html::App {}
}
[[[CODE_BLOCK_e1532b40-250e-498d-b51e-0ccad078829b]]]rust, no_run
{{#include src/doc_examples/boolean_attribute.rs:boolean_attribute}}
[[[CODE_BLOCK_cf66f73e-74e0-428e-b9a2-4a33c950dc81]]]inject-dioxus
DemoFrame {
	boolean_attribute::App {}
}
[[[CODE_BLOCK_397e4626-2e60-4f82-ad16-6896f141c299]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:formatting}}
[[[CODE_BLOCK_4457f008-fefe-4d44-82f8-7e94c4002e96]]]inject-dioxus
DemoFrame {
	rsx_overview::Formatting {}
}
[[[CODE_BLOCK_4fe4b376-314c-496b-9169-67afac38e105]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:children}}
[[[CODE_BLOCK_713a265c-6bc1-4d48-972d-3a8f5550a79d]]]inject-dioxus
DemoFrame {
	rsx_overview::Children {}
}
[[[CODE_BLOCK_64f76f64-1c28-4e55-8a25-d37d6af007bf]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:manyroots}}
[[[CODE_BLOCK_b9956d13-130e-4801-8572-93af7864f2f6]]]inject-dioxus
DemoFrame {
	rsx_overview::ManyRoots {}
}
[[[CODE_BLOCK_4b51a6e3-7450-4f3a-8f67-db669bf9f314]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:expression}}
[[[CODE_BLOCK_e1e8fb3d-f1bd-42e5-b419-53295cebb4d8]]]inject-dioxus
DemoFrame {
	rsx_overview::Expression {}
}
[[[CODE_BLOCK_0f13420d-ce41-406c-b251-d4be1ce95fbf]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:loops}}
[[[CODE_BLOCK_2f6d3588-5d45-4c37-a67b-f573175ac297]]]inject-dioxus
DemoFrame {
	rsx_overview::Loops {}
}
[[[CODE_BLOCK_b5f7bb88-8472-4063-8032-6b0f55dd1f6c]]]rust, no_run
{{#include src/doc_examples/rsx_overview.rs:ifstatements}}
[[[CODE_BLOCK_fb8a0609-598b-492b-82ca-cac4fa0f77eb]]]inject-dioxus
DemoFrame {
	rsx_overview::IfStatements {}
}
```