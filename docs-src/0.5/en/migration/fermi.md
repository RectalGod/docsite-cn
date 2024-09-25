# Fermi

在 Dioxus 0.5 中，Fermi 原子已被全局信号取代，并包含在主 Dioxus 库中。


新的全局信号可以直接使用，无需钩子，并包括额外的功能，例如全局备忘录。

Dioxus 0.4：
```rust
use dioxus::prelude::*;
use fermi::*;

static NAME: Atom<String> = Atom(|_| "world".to_string());
static NAMES: AtomRef<Vec<String>> = AtomRef(|_| vec!["world".to_string()]);

fn app(cx: Scope) -> Element {
    use_init_atom_root(cx);
    let set_name = use_set(cx, &NAME);
	let names = use_atom_ref(cx, &NAMES);

    cx.render(rsx! {
        button {
			onclick: move |_| set_name("dioxus".to_string()),
			"reset name"
		}
		"{names.read():?}"
    })
}
```

Dioxus 0.5：
```rust
{{#include src/doc_examples/migration_fermi.rs:intro}}
```

## 备忘录

Dioxus 0.5 引入了全局备忘录，可用于全局存储计算值。

```rust
{{#include src/doc_examples/migration_fermi.rs:memos}}
```