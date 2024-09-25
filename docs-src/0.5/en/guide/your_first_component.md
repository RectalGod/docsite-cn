# 你的第一个组件

本章将教你如何创建一个[Component](../reference/components.md)，它显示指向 Hacker News 上文章的链接。

## 设置

> 在开始本指南之前，请确保你已安装 Dioxus CLI 以及你所选平台所需的任何依赖项，如[getting started](../getting_started/index.md)指南中所述。

首先，让我们为我们的 Hacker News 应用创建一个新项目。我们可以使用 CLI 创建一个新项目。你可以选择你想要的平台，或者查看入门指南以获取有关每个选项的更多信息。如果你不确定尝试哪个平台，我们建议从 Web 或桌面开始：

```sh
dx new
```

模板包含一些样板代码来帮助你入门。在本指南中，我们将从头开始重建一些代码，以便于学习。你可以清空`src/main.rs`文件。我们将在下一节中添加新代码。

接下来，让我们设置我们的依赖项。我们需要设置一些依赖项来使用 Hacker News API：

```sh
cargo add chrono --features serde
cargo add futures
cargo add reqwest --features json
cargo add serde --features derive
cargo add serde_json
cargo add async_recursion
```

## 描述 UI

现在，我们可以定义如何显示文章。Dioxus 是一个*声明式*框架。这意味着我们不是告诉 Dioxus 要做什么（例如“创建一个元素”或“将颜色设置为红色”），而是简单地*声明*我们希望 UI 的外观。

要声明你希望 UI 的外观，你需要使用`rsx`宏。让我们创建一个``main``函数和一个``App``组件来显示关于我们故事的信息：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_v1}}
```

现在，如果你运行你的应用程序，你应该看到类似这样的内容：

```inject-dioxus
DemoFrame {
	hackernews_post::story_v1::App {}
}
```

> RSX 类似于 HTML。因此，你需要了解一些 HTML 才能使用 Dioxus。
> 
> 以下是一些资源可以帮助你开始学习 HTML：
> - [MDN HTML Guide](https://developer.mozilla.org/en-US/docs/Learn/HTML)
> - [W3 Schools HTML Tutorial](https://www.w3schools.com/html/default.asp)
> 
> 除了 HTML 之外，Dioxus 还使用 CSS 来为应用程序设置样式。你可以使用传统的 CSS（本指南中使用的）或使用像[tailwind CSS](https://tailwindcss.com/docs/installation):
> - [MDN Traditional CSS Guide](https://developer.mozilla.org/en-US/docs/Learn/HTML)
> - [W3 Schools Traditional CSS Tutorial](https://www.w3schools.com/css/default.asp)
> - [Tailwind tutorial](https://tailwindcss.com/docs/installation) (与 [Tailwind setup example](https://github.com/DioxusLabs/dioxus/tree/v0.5/examples/tailwind) 一起使用)
> 
> 如果你有现有的 html 代码，你可以使用 [translate](../CLI/translate.md) 命令将其转换为 RSX。或者如果你更喜欢编写 html，你可以使用 [html! macro](https://github.com/DioxusLabs/dioxus-html-macro) 在代码中直接编写 html。

## 动态文本

让我们扩展我们的`App`组件，以包括故事标题、作者、评分、发布时间和评论数量。我们可以在渲染宏中插入动态文本，方法是在`{}`中插入变量（这类似于[println!](https://doc.rust-lang.org/std/macro.println.html)宏中的格式）：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_v2}}
```

```inject-dioxus
DemoFrame {
	hackernews_post::story_v2::App {}
}
```

## 创建元素

接下来，让我们将我们的文章描述包装在一个[`div`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div) 中。你可以在 Dioxus 中创建 HTML 元素，方法是在元素名称后面加上一个`{`，在元素的最后一个子节点后面加上一个`}`：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_v3}}
```

```inject-dioxus
DemoFrame {
	hackernews_post::story_v3::App {}
}
```

> 你可以在[rsx reference](../reference/rsx.md)中了解更多关于元素的信息。

## 设置属性

接下来，让我们在文章列表周围添加一些填充，使用一个属性。

属性（以及[listeners](../reference/event_handlers.md)) 修改它们所附加元素的行为或外观。它们在任何子节点之前，在`{}`方括号内指定，使用`name: value`语法。你可以像对文本节点一样格式化属性中的文本：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_v4}}
```

```inject-dioxus
DemoFrame {
	hackernews_post::story_v4::App {}
}
```

> 注意：在[`dioxus-html`](https://docs.rs/dioxus-html/latest/dioxus_html/) 中定义的所有属性都遵循 snake_case 命名约定。它们将它们的`snake_case`名称转换为 HTML 的`camelCase`属性。

> 注意：样式可以直接在`style:`属性之外使用。在上面的例子中，`padding: "0.5rem"` 被转换为 `style="padding: 0.5rem"`。

> 你可以在[attribute reference](../reference/rsx.md)中了解更多关于元素的信息。

## 创建组件

就像你不会想在一个单一的、很长的、`main` 函数中编写一个复杂的程序一样，你也不应该在一个单一的`App` 函数中构建一个复杂的 UI。相反，你应该将应用程序的功能分解成称为组件的逻辑部分。

组件是一个 Rust 函数，以 UpperCamelCase 命名，它接受一个 props 参数，并返回一个`Element`，描述了它想要渲染的 UI。事实上，我们的`App` 函数就是一个组件！

让我们将我们的故事描述拉到一个新的组件中：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_v5}}
```

我们可以像渲染元素一样渲染我们的组件，在组件名称后面加上`{}`。让我们修改我们的`App`组件，以渲染我们新的 StoryListing 组件：

```rust
{{#include src/doc_examples/hackernews_post.rs:app_v5}}
```

```inject-dioxus
DemoFrame {
	hackernews_post::story_v5::App {}
}
```

> 你可以在[component reference](../reference/components.md)中了解更多关于元素的信息。

## 创建 Props

就像你可以向函数传递参数或向元素传递属性一样，你也可以向组件传递 props，以自定义其行为！

我们可以在组件被渲染时定义组件可以接受的参数（称为`Props`)，方法是在函数定义之前添加`#[component]`宏，并添加额外的函数参数。

目前，我们的`StoryListing`组件始终渲染同一个故事。我们可以对其进行修改，使其接受一个要渲染的故事作为 prop。


我们还将定义什么是文章，并包括如何使用[serde](https://serde.rs)将我们的文章转换为不同的格式，以及如何从不同的格式转换回来。这将在后面的章节中与 Hacker News API 一起使用：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_v6}}
```

确保也添加[serde](https://serde.rs)作为依赖项：

```bash
cargo add serde --features derive
cargo add serde_json
```

我们还将使用[chrono](https://crates.io/crates/chrono) crate 来提供用于处理 Hacker News API 中的时间数据的实用程序：
```bash
cargo add chrono --features serde
```


现在，让我们修改`App`组件，以将故事传递给我们的`StoryListing`组件，就像我们在元素上设置属性一样：

```rust
{{#include src/doc_examples/hackernews_post.rs:app_v6}}
```

```inject-dioxus
DemoFrame {
	hackernews_post::story_v6::App {}
}
```

> 你可以在[Props reference](../reference/component_props.md)中了解更多关于 Props 的信息。

## 清理我们的界面

最后，通过组合元素和属性，我们可以使我们的文章列表更具吸引力：

到目前为止的完整代码：

```rust
{{#include src/doc_examples/hackernews_post.rs:story_final}}
```

```inject-dioxus
DemoFrame {
	hackernews_post::story_final::App {}
}
```