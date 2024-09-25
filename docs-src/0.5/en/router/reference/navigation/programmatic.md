# 程序化导航

有时我们希望应用程序在没有用户点击链接的情况下导航到另一个页面。这被称为程序化导航。

## 使用导航器

我们可以使用 ``navigator`` 函数获取导航器，该函数返回一个 ``Navigator``。

我们可以使用 ``Navigator`` 触发四种不同的导航类型：

- ``push`` 将导航到目标。它的工作方式类似于常规的锚点标签。
- ``replace`` 的工作方式类似于 ``push``，不同之处在于它替换当前的历史记录条目而不是添加新的条目。这意味着无法使用浏览器的后退按钮恢复之前的页面。
- ``Go back`` 的工作方式类似于浏览器的后退按钮。
- ``Go forward`` 的工作方式类似于浏览器的前进按钮。

```rust
use dioxus_router::prelude::*;

fn main() {
    dioxus::web::launch(App);
}

#[derive(Props)]
struct App {
    navigator: Navigator,
}

fn App(ctx: &Context, props: &App) -> Element {
    let navigator = props.navigator.clone();
    ctx.render(rsx! {
        // ...
        button {
            onclick: move |_| {
                navigator.push("/home");
            }
            "Go to Home"
        }
        button {
            onclick: move |_| {
                navigator.replace("/about");
            }
            "Replace with About"
        }
        // ...
    })
}
```

您可能已经注意到，与 ``Link`` 一样，``Navigator`` 的 ``push`` 和 ``replace`` 函数接受一个 ``NavigationTarget``. 这意味着我们可以使用 ``Internal`` 或 ``External`` 目标。

## 外部导航目标

与 ``Link`` 不同，``Navigator`` 无法依赖浏览器（或 Webview）通过生成的锚点元素来处理外部目标的导航。

这意味着，在某些情况下，外部目标的导航可能会失败。


``Link``: https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Link.html
``NavigationTarget``: https://docs.rs/dioxus-router/latest/dioxus_router/navigation/enum.NavigationTarget.html
``Navigator``: https://docs.rs/dioxus-router/latest/dioxus_router/prelude/struct.Navigator.html