# 钩子

Dioxus 现在使用信号作为其状态管理的支撑。信号是一个更智能、更灵活的 `use_ref` 钩子版本。信号现在支持 Dioxus 中的许多钩子，以提供更一致和灵活的 API。

### 状态钩子

状态钩子现在由信号支持。 `use_state`, `use_ref`, 和 `use_shared_state` 已被 `use_signal` 钩子替换。`use_signal` 钩子是 `use_ref` 钩子的更灵活和强大的版本，具有更智能的范围，只有在该信号在范围内读取时才会订阅该信号。您可以在 [State Migration](state.md) 指南中了解更多关于 `use_signal` 钩子的信息。

### 异步钩子

`use_future` 钩子已被 `use_resource` 钩子替换。`use_resource` 会自动订阅闭包中读取的任何信号，而不是使用依赖项元组。

Dioxus 0.4：

```rust
fn MyComponent(cx: Scope) -> Element {
	let state = use_state(cx, || 0);
	let my_resource = use_future(cx, (**state,), |(state,)| async move {
		// start a request that depends on the state
		println!("{state}");
	});
	render! {
		"{state}"
	}
}
```

Dioxus 0.5：

```rust
{{#include src/doc_examples/migration_hooks.rs:use_resource}}
```

### 依赖项

一些钩子，包括 `use_effect` 和 `use_resource`，现在接受一个带有自动订阅的单个闭包，而不是依赖项元组。您可以在 [Hook Migration](hooks.md) 指南中了解更多关于 `use_resource` 钩子的信息。

Dioxus 0.4：

```rust
fn HasDependencies(cx: Scope) -> Element {
	let state = use_state(cx, || 0);
	let my_resource = use_resource(cx, (**state,), |(state,)| async move {
		println!("{state}");
	});
	let state_plus_one = use_memo(cx, (**state,), |(state,)| {
		state() + 1
	});
	render! {
		"{state_plus_one}"
	}
}
```

Dioxus 0.5：

```rust
{{#include src/doc_examples/migration_hooks.rs:dependencies}}
```