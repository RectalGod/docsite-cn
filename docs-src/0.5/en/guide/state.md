# 交互性

本章我们将添加一个预览功能，当您将鼠标悬停在文章上或将焦点放在链接上时，预览功能就会出现。

## 创建预览

首先，我们将应用程序拆分为屏幕左侧的“故事”组件和屏幕右侧的“预览”组件：

```rust
{{#include src/doc_examples/hackernews_state.rs:app_v1}}
```

```inject-dioxus
DemoFrame {
    hackernews_state::app_v1::App {}
}
```

## 事件处理程序

接下来，我们需要检测用户何时将鼠标悬停在某个部分或将焦点放在链接上。我们可以使用 [event listener](../reference/event_handlers.md) 来监听悬停和聚焦事件。

事件处理程序类似于常规属性，但它们的名字通常以 `on` 开头，并且它们接受闭包作为值。当监听的事件触发时，闭包就会被调用。当事件触发时，关于事件的信息会通过 [Event](https://docs.rs/dioxus/latest/dioxus/prelude/struct.Event.html) 结构传递给闭包。

让我们在 `StoryListing` 组件中创建一个 [`onmouseenter`](https://docs.rs/dioxus/latest/dioxus/events/fn.onmouseenter.html) 事件监听器：

```rust
{{#include src/doc_examples/hackernews_state.rs:story_listing_listener}}
```

> 您可以在 [Event Handler reference](../reference/event_handlers.md) 中阅读更多关于事件处理程序的信息

## 状态

到目前为止，我们的组件还没有像普通的 Rust 函数那样拥有状态。为了让应用程序在我们将鼠标悬停在链接上时发生改变，我们需要在应用程序的根部创建状态来存储当前悬停的链接。

您可以在 Dioxus 中使用钩子创建状态。钩子是您在组件中按特定顺序调用的 Rust 函数，它们为组件添加了额外的功能。

在本例中，我们将使用 `use_context_provider` 和 `use_context` 钩子：

- 您可以向 `use_context_provider` 提供一个闭包，该闭包确定共享状态的初始值，并将该值提供给所有子组件。
- 然后，您可以使用 `use_context` 钩子在 `Preview` 和 `StoryListing` 组件中读取和修改该状态。
- 当值更新时，`Signal` 将导致组件重新渲染，并为您提供新的值。

> 注意：如果您只在一个组件中使用状态，则应该优先使用局部状态钩子，如 use_signal 或 use_signal_sync。因为我们在多个组件中使用状态，所以可以使用 [global state pattern](../reference/context.md)。

```rust
{{#include src/doc_examples/hackernews_state.rs:shared_state_app}}
```

```rust
{{#include src/doc_examples/hackernews_state.rs:shared_state_stories}}
```

```rust
{{#include src/doc_examples/hackernews_state.rs:shared_state_preview}}
```

```inject-dioxus
DemoFrame {
    hackernews_state::App {}
}
```

> 您可以在 [Hooks reference](../reference/hooks.md) 中阅读更多关于钩子的信息

### 钩子的规则

钩子是管理 Dioxus 中状态的一种强大方法，但您需要遵循一些规则以确保它们按预期工作。Dioxus 使用您调用钩子的顺序来区分钩子。由于您调用钩子的顺序很重要，因此您必须遵循以下规则：

1. 钩子只能在组件或其他钩子中使用（我们将在后面介绍）。
2. 在每次调用组件函数时：
   1. 必须调用相同的钩子。
   2. 必须按相同的顺序调用。
3. 钩子的名字应该以 `use_` 开头，这样您就不会意外地将它们与常规函数混淆。

这些规则意味着您无法用钩子做某些事情：

#### 在条件语句中使用钩子
```rust
{{#include src/doc_examples/hooks_bad.rs:conditional}}
```

#### 在闭包中使用钩子
```rust
{{#include src/doc_examples/hooks_bad.rs:closure}}
```

#### 在循环中使用钩子
```rust
{{#include src/doc_examples/hooks_bad.rs:loop}}
```