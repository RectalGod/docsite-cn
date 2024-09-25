# 如何升级到 Dioxus 0.5

本指南将概述 `0.4` 和 `0.5` 版本之间的 API 变化。

`0.5` 包含对钩子、道具和全局状态的重大更改。

## 快速参考

以下是更改的快速参考：

### 范围

Dioxus 0.4:
```rust
fn app(cx: Scope) -> Element {
    cx.use_hook(|| {
        /*...*/
    });
    cx.provide_context({
        /*...*/
    });
    cx.spawn(async move {
        /*...*/
    });
    cx.render(rsx! {
        /*...*/
    })
}
```
Dioxus 0.5:
```rust
{{#include src/doc_examples/migration.rs:scope}}
```

### 道具

Dioxus 0.4:
```rust
#[component]
fn Comp(cx: Scope, name: String) -> Element {
    // You pass in an owned prop, but inside the component, it is borrowed (name is the type &String inside the function)
    let owned_name: String = name.clone();

    cx.render(rsx! {
        "Hello {owned_name}"
        BorrowedComp {
            "{name}"
        }
        ManualPropsComponent {
            name: name
        }
    })
}

#[component]
fn BorrowedComp<'a>(cx: Scope<'a>, name: &'a str) -> Element<'a> {
    cx.render(rsx! {
        "Hello {name}"
    })
}

#[derive(Props, PartialEq)]
struct ManualProps {
    name: String
}

fn ManualPropsComponent(cx: Scope<ManualProps>) -> Element {
    cx.render(rsx! {
        "Hello {cx.props.name}"
    })
}
```

Dioxus 0.5:
```rust
{{#include src/doc_examples/migration.rs:props}}
```

您可以在 [Props Migration](props.md) 指南中了解更多关于新道具 API 的信息。

### Futures

Dioxus 0.4:
```rust
use_future((dependency1, dependency2,), move |(dependency1, dependency2,)| async move {
	/*use dependency1 and dependency2*/
});
```
Dioxus 0.5:
```rust
{{#include src/doc_examples/migration.rs:futures}}
```

在 [Hook Migration](hooks.md) 指南中阅读有关 `use_resource` 钩子的更多信息。

### 状态钩子

Dioxus 0.4:
```rust
let copy_state = use_state(cx, || 0);
let clone_local_state = use_ref(cx, || String::from("Hello"));
use_shared_state_provider(cx, || String::from("Hello"));
let clone_shared_state = use_shared_state::<String>(cx);

let copy_state_value = **copy_state;
let clone_local_state_value = clone_local_state.read();
let clone_shared_state_value = clone_shared_state.read();

cx.render(rsx!{
	"{copy_state_value}"
	"{clone_shared_state_value}"
	"{clone_local_state_value}"
	button {
		onclick: move |_| {
			copy_state.set(1);
			*clone_local_state.write() = "World".to_string();
			*clone_shared_state.write() = "World".to_string();
		},
		"Set State"
	}
})
```

Dioxus 0.5:

```rust
{{#include src/doc_examples/migration.rs:state}}
```

在 [State Migration](state.md) 指南中阅读有关 `use_signal` 钩子的更多信息。

### Fermi

Dioxus 0.4:
```rust
use dioxus::prelude::*;
use fermi::*;

static NAME: Atom<String> = Atom(|_| "world".to_string());

fn app(cx: Scope) -> Element {
    use_init_atom_root(cx);
    let name = use_read(cx, &NAME);

    cx.render(rsx! {
        div { "hello {name}!" }
        Child {}
        ChildWithRef {}
    })
}

fn Child(cx: Scope) -> Element {
    let set_name = use_set(cx, &NAME);

    cx.render(rsx! {
        button {
            onclick: move |_| set_name("dioxus".to_string()),
            "reset name"
        }
    })
}

static NAMES: AtomRef<Vec<String>> = AtomRef(|_| vec!["world".to_string()]);

fn ChildWithRef(cx: Scope) -> Element {
    let names = use_atom_ref(cx, &NAMES);

    cx.render(rsx! {
        div {
            ul {
                names.read().iter().map(|f| rsx!{
                    li { "hello: {f}" }
                })
            }
            button {
                onclick: move |_| {
                    let names = names.clone();
                    cx.spawn(async move {
                        names.write().push("asd".to_string());
                    })
                },
                "Add name"
            }
        }
    })
}
```

Dioxus 0.5:
```rust
{{#include src/doc_examples/migration.rs:fermi}}
```

您可以在 [Fermi migration guide](fermi.md) 中阅读有关全局信号的更多信息。