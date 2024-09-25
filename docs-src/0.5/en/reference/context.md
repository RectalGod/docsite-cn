# 共享状态

通常情况下，多个组件需要访问同一个状态。根据您的需求，有几种方法可以实现这一点。

## 状态提升

在组件之间共享状态的一种方法是将状态“提升”到最近的共同祖先。这意味着在父组件中放置 `use_signal` 钩子，并将所需的值作为道具传递下去。

假设我们要构建一个表情包编辑器。我们希望有一个输入框来编辑表情包的标题，还要有一个显示带有标题的表情包的预览。从逻辑上讲，表情包和输入框是两个独立的组件，但它们需要访问同一个状态（当前标题）。

> 当然，在这个简单的例子中，我们可以在一个组件中编写所有内容，但将所有内容拆分成更小的组件会更好，这样代码会更可重用、更易于维护且性能更高（这对大型、复杂的应用程序来说更为重要）。

我们从一个 `Meme` 组件开始，它负责渲染带有给定标题的表情包：

```rust, no_run
{{#include src/doc_examples/meme_editor.rs:meme_component}}
[[[CODE_BLOCK_2d6646ed-4b14-4f56-9c1f-4ad91f041f70]]]rust, no_run
{{#include src/doc_examples/meme_editor.rs:caption_editor}}
[[[CODE_BLOCK_a084d20f-665c-4ffb-80e3-3a9094b2c3a0]]]rust, no_run
{{#include src/doc_examples/meme_editor.rs:meme_editor}}
[[[CODE_BLOCK_5379f01b-94ce-4663-ae18-b037d821e962]]]rust, no_run
{{#include src/doc_examples/meme_editor_dark_mode.rs:DarkMode_struct}}
[[[CODE_BLOCK_3cfceaf1-ed7c-4bb8-bc56-e1ab5e70e215]]]rust, no_run
{{#include src/doc_examples/meme_editor_dark_mode.rs:context_provider}}
[[[CODE_BLOCK_ef9ccbc0-36b9-4b09-84e0-6e88c766c6a8]]]rust, no_run
{{#include src/doc_examples/meme_editor_dark_mode.rs:use_context}}
[[[CODE_BLOCK_52c81f7e-bd6c-4a81-8218-7cb22c2d21e2]]]rust, no_run
{{#include src/doc_examples/meme_editor_dark_mode.rs:toggle}}
```