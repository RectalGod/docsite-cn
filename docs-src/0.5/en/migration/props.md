# 属性迁移

在 Dioxus 0.4 中，属性通过作用域传递给组件。在 Dioxus 0.5 中，属性直接通过属性结构体传递给组件。

## 拥有属性

属性是通过作用域的生存期借用的。属性在每次渲染时都会被克隆，并作为拥有值传递给组件。

Dioxus 0.4：
```rust
#[component]
fn Comp(cx: Scope, name: String) -> Element {
    // You pass in an owned prop, but inside the component, it is borrowed (name is the type &String inside the function)
    let owned_name: String = name.clone();

    cx.render(rsx! {
        "Hello {owned_name}"
    })
}
```
Dioxus 0.5：
```rust
{{#include src/doc_examples/migration_props.rs:owned_props}}
```

因为属性在每次渲染时都会被克隆，所以建议将属性设置为 `Copy`。你可以通过在属性结构体中接受 ``ReadOnlySignal<T>`` 而不是 ``T`` 来轻松地将字段设置为 `Copy`：

```rust
{{#include src/doc_examples/migration_props.rs:copy_props}}
```

## 借用属性

在 Dioxus 0.5 中，借用属性已被移除。如果你的属性是从状态借用的，映射信号可以类似于借用属性。

Dioxus 0.4：
```rust
fn Parent(cx: Scope) -> Element {
    let state = use_state(cx, || (1, "World".to_string()));
    rsx! {
        BorrowedComp {
            name: &state.get().1
        }
    }
}

#[component]
fn BorrowedComp<'a>(cx: Scope<'a>, name: &'a str) -> Element<'a> {
    rsx! {
        "Hello {name}"
    }
}
```

Dioxus 0.5：
```rust
{{#include src/doc_examples/migration_props.rs:borrowed_props}}
```

## 手动属性

在 Dioxus 0.5 中，手动属性结构体除了需要派生 ``Props`` 和 ``PartialEq`:` 之外，还需要派生 ``Clone``:

Dioxus 0.4：
```rust
#[derive(Props, PartialEq)]
struct ManualProps {
    name: String,
}

// Functions accept the props directly instead of the scope
fn ManualPropsComponent(cx: Scope<ManualProps>) -> Element {
    render! {
        "Hello {cx.props.name}"
    }
}
```

Dioxus 0.5：
```rust
{{#include src/doc_examples/migration_props.rs:manual_props}}
```