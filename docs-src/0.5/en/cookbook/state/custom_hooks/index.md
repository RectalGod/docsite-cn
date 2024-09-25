# 自定义钩子

钩子是封装业务逻辑的好方法。如果现有的钩子都不适合你的问题，你可以编写自己的钩子。

编写自定义钩子时，你可以创建一个以 `use_` 开头的函数，并接受任何你需要的参数。然后，你可以使用 `use_hook` 方法创建一个钩子，该钩子将在组件第一次渲染时被调用。

## 组合钩子

为了避免重复，你可以将基于现有钩子的业务逻辑封装起来，创建新的钩子。

例如，如果许多组件需要访问一个 `AppSettings` 结构体，你可以创建一个“快捷”钩子：

```rust
{{#include src/doc_examples/hooks_composed.rs:wrap_context}}
```

或者，如果你想使用存储 API 包装一个在重新加载后保留的钩子，你可以基于 use_signal 钩子构建来处理可变状态：

```rust
{{#include src/doc_examples/hooks_composed.rs:use_storage}}
```

## 自定义钩子逻辑

你可以使用 [`use_hook`](https://docs.rs/dioxus/latest/dioxus/prelude/fn.use_hook.html) 来构建自己的钩子。事实上，所有标准钩子都是基于此构建的！

`use_hook` 接受一个用于初始化钩子的闭包。它只会在组件第一次渲染时运行。该闭包的返回值将用作钩子的值 - Dioxus 会获取它，并在组件存活期间存储它。在每次渲染时（不仅仅是第一次！），你将获得对该值的引用。

> 注意：你可以使用 `use_on_destroy` 钩子来清理组件销毁时钩子使用的任何资源。

在初始化闭包内，你通常会调用其他 `cx` 方法。例如：

- `use_signal` 钩子在钩子值中跟踪状态，并使用 [`schedule_update`](https://docs.rs/dioxus/latest/dioxus/prelude/fn.schedule_update.html) 在状态更改时让 Dioxus 重新渲染组件。

以下是用简化的方式实现 `use_signal` 钩子：

```rust
{{#include src/doc_examples/hooks_custom_logic.rs:use_signal}}
```

- `use_context` 钩子调用 [`consume_context`](https://docs.rs/dioxus/latest/dioxus/prelude/fn.consume_context.html) （这在每次渲染时调用会很昂贵）来从组件获取一些上下文

以下是用简化的方式实现 `use_context` 和 `use_context_provider` 钩子：

```rust
{{#include src/doc_examples/hooks_custom_logic.rs:use_context}}
```