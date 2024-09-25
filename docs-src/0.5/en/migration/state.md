# 状态迁移

`use_state` 和 `use_ref` 钩子已被 `use_signal` 钩子取代。`use_signal` 钩子是 `use_ref` 钩子的更灵活、更强大的版本，它具有更智能的作用域，仅在作用域内读取信号时才会订阅该信号。

使用 `use_state`, 如果您有以下代码：
```rust
fn Parent(cx: Scope) -> Element {
	let state = use_state(cx, || 0);

	render! {
		Child {
			state: state.clone()
		}
	}
}

#[component]
fn Child(cx: Scope, state: UseState<i32>) -> Element {
	render! {
		"{state}"
	}
}
```

即使只有子组件在使用状态，父组件也会在每次状态改变时重新渲染。使用新的 `use_signal` 钩子，只有当父组件内部的状态发生改变时，父组件才会重新渲染：

```rust
{{#include src/doc_examples/migration_state.rs:use_signal}}
```
只有子组件会在状态改变时重新渲染，因为只有子组件在读取状态。

## 基于上下文的狀態

`use_shared_state_provider` 和 `use_shared_state` 钩子已被使用 `use_context_provider` 和 `use_context` 钩子以及 `Signal` 取代：

```rust
{{#include src/doc_examples/migration_state.rs:context_signals}}
```

信号足够智能，可以处理订阅正确的范围，而无需特殊的共享状态钩子。

## 选择退出订阅

一些状态钩子，包括 `use_shared_state` 和 `use_ref` 钩子，在 `0.4` 中有一个名为 `write_silent` 的函数。此函数允许您更新状态，而不会触发任何订阅者的重新渲染。此函数已在 `0.5` 中删除。

相反，您可以使用 `peek` 函数读取信号的当前值，而无需订阅它。这反转了订阅模型，因此您可以选择退出订阅信号，而不是选择所有订阅者退出更新：

```rust
{{#include src/doc_examples/migration_state.rs:peek}}
```

`peek` 使您能够更精细地控制何时订阅信号。这对性能优化和更新状态而不重新渲染组件很有用。

## 全局状态

在 `0.4` 中，fermi 容器提供了一个单独的全局状态 API，称为原子。在 `0.5` 中，`Signal` 类型已扩展以提供全局状态 API。您可以使用 `Signal::global` 函数创建全局信号：

```rust
{{#include src/doc_examples/migration_state.rs:global_signals}}
```

您可以在 [Fermi migration guide](fermi.md) 中阅读有关全局信号的更多信息。