# 生成 Future

``use_resource`` 和 ``use_coroutine`` 钩子在您想无条件生成 Future 时很有用。但是，有时您可能只想在响应事件（例如鼠标点击）时生成 Future。例如，假设您需要在用户点击“登录”按钮时发送请求。为此，您可以使用 ``spawn``:

```rust
{{#include src/doc_examples/spawn.rs:spawn}}
```

```rust
use dioxus::prelude::*;
use dioxus_web::WebRenderer;

fn main() {
    dioxus_web::launch(App);
}

#[derive(Props)]
struct AppProps {}

#[allow(unused)]
fn App(cx: Scope<AppProps>) -> Element {
    cx.use_future(|_| async move {
        // 这里可以放你的异步代码
        println!("Hello from a spawned Future!");
        // 这里可以返回一个值
        "Hello world!"
    });

    cx.render(rsx! {
        button {
            onclick: move |_| {
                cx.use_future(|_| async move {
                    // 在按钮点击时生成一个新的 Future
                    println!("Button was clicked!");
                })
            }
            "Click me!"
        }
    })
}
```

```inject-dioxus
DemoFrame {
    spawn::App {}
}
```

> 注意：``spawn`` 将始终生成一个 _新的_ Future。您很可能不希望在每次渲染时调用它。

调用 ``spawn`` 将为您提供一个 ``JoinHandle``, 它允许您取消或暂停 Future。

## 生成 Tokio 任务

有时，您可能希望生成一个需要多个线程或与可能阻塞应用程序代码的硬件进行通信的后台任务。在这些情况下，我们可以直接从 Future 生成 Tokio 任务。对于 Dioxus-Desktop，您的任务将被生成到 Tokio 的多线程运行时上：

```rust
{{#include src/doc_examples/spawn.rs:tokio}}
```

```rust
use dioxus::prelude::*;
use dioxus_web::WebRenderer;
use tokio::runtime::Runtime;

fn main() {
    let rt = Runtime::new().unwrap();

    dioxus_web::launch_with_runtime(rt, App);
}

#[derive(Props)]
struct AppProps {}

#[allow(unused)]
fn App(cx: Scope<AppProps>) -> Element {
    cx.use_future(|_| async move {
        // 在后台生成一个 Tokio 任务
        tokio::spawn(async move {
            // 在任务中执行异步代码
            println!("Hello from a spawned Tokio task!");
        });
    });

    cx.render(rsx! {
        div { "Hello from Dioxus!" }
    })
}
```