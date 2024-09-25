# 错误处理

Rust 在 Web 开发中的一个优势是，它总是让你知道错误可能发生在哪里，并强制你处理它们。

然而，在本指南中我们并没有讨论错误处理！ 在本章中，我们将介绍一些处理错误的策略，以确保你的应用程序永远不会崩溃。

## 最简单的 - 返回 None

敏锐的观察者可能已经注意到 `Element` 实际上是 `Option<VNode>` 的类型别名。你不需要知道 `VNode` 是什么，但重要的是要认识到，我们实际上可以完全不返回任何东西：

```rust
{{#include src/doc_examples/error_handling.rs:none}}
```

这让我们可以为我们认为 *不应该* 失败的操作添加一些语法糖，但我们仍然没有足够的信心去 "解包"。

> `Option<VNode>` 的性质可能会在将来随着 `try` 特征的升级而改变。

```rust
{{#include src/doc_examples/error_handling.rs:try_hook}}
```

## 结果的提前返回

因为 Rust 无法使用现有的 try 基础设施接受 Options 和 Results，所以你需要手动处理 Results。 这可以通过将它们转换为 Options 或显式地处理它们来完成。 如果你选择将你的 Result 转换为 Option 并使用 `?` 来冒泡，请记住，如果你确实遇到了错误，你将丢失错误信息，并且该组件将不会呈现任何内容。

```rust
{{#include src/doc_examples/error_handling.rs:try_result_hook}}
```

注意，虽然 Dioxus 中的钩子不喜欢在条件语句或循环中被调用，但它们 *可以* 提前返回。 提前返回错误状态是处理错误的一种完全有效的方法。

## 匹配结果

在 Dioxus 中处理错误的下一个 "最佳" 方法是在本地匹配错误。 这是处理错误最健壮的方法，但它无法扩展到单个组件之外的架构。

为此，我们只需在我们的组件中内置一个错误状态：

```rust
{{#include src/doc_examples/error_handling.rs:use_error}}
```

每当我们执行生成错误的操作时，我们都会设置该错误状态。 然后，我们可以通过多种方式（提前返回、返回 Element 等）匹配错误。

```rust
{{#include src/doc_examples/error_handling.rs:match_error}}
```

## 通过组件传递错误状态

如果你正在处理几个嵌套程度不高的组件，你可以将错误处理程序传递给子组件。

```rust
{{#include src/doc_examples/error_handling.rs:match_error_children}}
```

与之前一样，我们的子组件可以在它们自己的操作期间手动设置错误。 这种模式的优势是我们可以很容易地将错误状态隔离到几个组件中，这使得我们的应用程序更可预测和健壮。

## 抛出错误

Dioxus 提供了一种更轻松的处理错误的方法：抛出错误。 抛出错误结合了错误状态和提前返回的最佳部分：你可以很容易地用 `?` 抛出错误，但你保留了有关错误的信息，以便你可以在父组件中处理它。

你可以在任何实现了 `Debug` 的 `Result` 类型上调用 `throw`，将其转换为错误状态，然后使用 `?` 在遇到错误时提前返回。 你可以用 `ErrorBoundary` 组件捕获错误状态，如果在任何子组件中抛出错误，该组件将呈现不同的组件。

```rust
{{#include src/doc_examples/error_handling.rs:throw_error}}
```

你甚至可以嵌套 `ErrorBoundary` 组件，以便在应用程序的不同级别捕获错误。

```rust
{{#include src/doc_examples/error_handling.rs:nested_throw}}
```

这种模式在你的代码生成不可恢复的错误时特别有用。 你可以优雅地捕获这些 "全局" 错误状态，而不会出现恐慌或自己处理每个错误的状态。