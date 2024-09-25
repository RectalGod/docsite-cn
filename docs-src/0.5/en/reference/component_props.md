# 组件 Props

就像你可以向函数传递参数或向元素传递属性一样，你也可以向组件传递 Props 来定制其行为！我们迄今为止看到的组件没有接受任何 Props - 所以让我们编写一些接受 Props 的组件。

## `#[derive(Props)]`

组件 Props 是一个用 `#[derive(PartialEq, Clone, Props)]` 标记的单个结构体。为了让组件接受 Props，其参数的类型必须为 `YourPropsStruct`。

示例：

```rust, no_run
{{#include src/doc_examples/component_owned_props.rs:Likes}}
[[[CODE_BLOCK_4210c148-e976-40eb-96b1-e334cc8a83fc]]]rust, no_run
{{#include src/doc_examples/component_owned_props.rs:App}}
[[[CODE_BLOCK_52fe42b9-466f-4087-a044-081d10afc217]]]inject-dioxus
DemoFrame {
    component_owned_props::App {}
}
[[[CODE_BLOCK_e7dfc985-2c60-43ea-8612-090262869876]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:OptionalProps}}
[[[CODE_BLOCK_bebbb73e-fc3a-4903-8110-1f98da217243]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:OptionalProps_usage}}
[[[CODE_BLOCK_794d7513-7c88-4257-bf30-ee56edc06484]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:ExplicitOption}}
[[[CODE_BLOCK_28dd69e5-a541-4781-bddf-94d3e62a9280]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:ExplicitOption_usage}}
[[[CODE_BLOCK_bce8c638-f703-46f4-9785-0a32bea70af4]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:DefaultComponent}}
[[[CODE_BLOCK_16004441-3f1c-4d8a-9d45-f987d84bd194]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:DefaultComponent_usage}}
[[[CODE_BLOCK_28f6c305-5407-43a3-870c-25beff2805ae]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:IntoComponent}}
[[[CODE_BLOCK_d5015ac6-0a63-4913-a347-fa796fe79694]]]rust, no_run
{{#include src/doc_examples/component_props_options.rs:IntoComponent_usage}}
[[[CODE_BLOCK_8a4448be-11cb-4d1a-b726-7204b26feda5]]]rust, no_run
#[derive(Props, Clone, PartialEq)]
struct TitleCardProps {
    title: String,
}

fn TitleCard(props: TitleCardProps) -> Element {
    rsx!{
        h1 { "{props.title}" }
    }
}
[[[CODE_BLOCK_24b0b2a8-6a7f-4764-95bd-f6ce14a2fd2d]]]rust, no_run
#[component]
fn TitleCard(title: String) -> Element {
    rsx!{
        h1 { "{title}" }
    }
}
[[[CODE_BLOCK_5408a91b-6c16-4986-9622-b02e9b40df6c]]]rust, no_run
{{#include src/doc_examples/component_element_props.rs:Clickable}}
[[[CODE_BLOCK_955a2819-8c42-42a8-8b54-b55aa35f7eae]]]rust, no_run
{{#include src/doc_examples/component_element_props.rs:Clickable_usage}}
[[[CODE_BLOCK_ef7db7fc-705c-4dd1-9ed8-12d011d7ba70]]]rust, no_run
{{#include src/doc_examples/component_children.rs:Clickable}}
[[[CODE_BLOCK_a65481d5-bf8a-4859-afb2-2d5666af44c1]]]rust, no_run
{{#include src/doc_examples/component_children.rs:Clickable_usage}}
[[[CODE_BLOCK_7bfc7a1a-be91-4556-a147-7b7e30a52e83]]]inject-dioxus
DemoFrame {
    component_children::App {}
}
```