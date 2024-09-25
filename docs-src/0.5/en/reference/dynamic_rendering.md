# 动态渲染

有时候你希望根据状态/属性渲染不同的内容。使用 Dioxus，只需使用 Rust 控制流描述你想要看到的内容——框架会在状态或属性发生变化时自动进行必要的更改！

## 条件渲染

要根据条件渲染不同的元素，你可以使用 ``if-else`` 语句：

```rust, no_run
{{#include src/doc_examples/conditional_rendering.rs:if_else}}
[[[CODE_BLOCK_429dd041-6ebf-4bb1-9d4f-1f2a96f4d188]]]inject-dioxus
DemoFrame {
  conditional_rendering::App {}
}
[[[CODE_BLOCK_ec130c63-4f25-452c-8b25-0c7ef9d9b3bd]]]rust, no_run
{{#include src/doc_examples/conditional_rendering.rs:if_else_improved}}
[[[CODE_BLOCK_a7790350-f4c7-4308-a7c2-5a61a7154e41]]]inject-dioxus
DemoFrame {
  conditional_rendering::LogInImprovedApp {}
}
[[[CODE_BLOCK_50623923-d62e-486f-b1c3-6216fef01d07]]]rust, no_run
{{#include src/doc_examples/component_children_inspect.rs:Clickable}}
[[[CODE_BLOCK_3ac194c5-e6ed-4a17-a8ea-6ff2a7c27c48]]]rust, no_run
{{#include src/doc_examples/conditional_rendering.rs:conditional_none}}
[[[CODE_BLOCK_f7cd995a-be77-4386-b8c5-d08e6a69064e]]]inject-dioxus
DemoFrame {
  conditional_rendering::LogInWarningApp {}
}
[[[CODE_BLOCK_75f9c996-56b5-42c0-be02-dc252fb2a152]]]rust, no_run
{{#include src/doc_examples/rendering_lists.rs:render_list}}
[[[CODE_BLOCK_0ffed4d2-5e53-4d1d-8459-01ffd28f2f2f]]]inject-dioxus
DemoFrame {
  rendering_lists::App {}
}
[[[CODE_BLOCK_f8c44836-5a50-4a56-b9f9-f3c3fe95effc]]]rust, no_run
{{#include src/doc_examples/rendering_lists.rs:render_list_for_loop}}
[[[CODE_BLOCK_9905c464-5e26-4fee-b291-7f9a59ca9902]]]inject-dioxus
DemoFrame {
  rendering_lists::AppForLoop {}
}
```

### ``key`` 属性

每次重新渲染列表时，Dioxus 都需要跟踪每个项目的位置，以确定需要对 UI 进行哪些更新。

例如，假设 ``CommentComponent`` 有一些状态——例如，用户在其中键入回复的字段。如果评论的顺序突然发生变化，Dioxus 需要将该状态正确地与同一评论关联——否则，用户最终会回复不同的评论！

为了帮助 Dioxus 跟踪列表项，我们需要将每个项目与一个唯一的键关联。在上面的示例中，我们动态生成了唯一的键。在实际应用中，键更有可能来自数据库 ID 等。从哪里获取键并不重要，只要它满足以下要求：

- 键在列表中必须是唯一的
- 相同的项目应该始终与相同的键关联
- 键应该相对较小（例如，将整个 `Comment` 结构转换为字符串将是一个非常糟糕的键），以便可以有效地比较它们

你可能很想将列表中项目的索引用作其键。如果你根本没有指定键，Dioxus 将使用它。只有在你能保证列表是恒定的情况下，这才可以接受——即没有重新排序、添加或删除。

> 请注意，如果你将键传递给已创建的组件，它不会将键作为属性接收。它仅用作 Dioxus 本身的提示。如果你的组件需要 ID，则必须将其作为单独的属性传递。