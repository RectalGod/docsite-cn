# 与服务器通信

`dioxus-fullstack` 提供了服务器函数，允许您从客户端调用服务器上的自动生成的 API，就像调用本地函数一样。

要创建服务器函数，只需在函数上添加 `#[server(YourUniqueType)]` 属性。该函数必须：

- 是一个异步函数
- 拥有参数和返回值，两者都实现序列化和反序列化（使用 [serde](https://serde.rs/)）。
- 返回一个 `Result`，其错误类型为 ServerFnError

您必须在主函数中将 `register` 调用传递给服务器宏的类型，然后启动服务器，以告知 Dioxus 有关服务器函数的信息。

让我们继续构建在 [getting started](../../getting_started/fullstack.md) 指南中创建的应用程序。我们将向应用程序添加一个服务器函数，该函数允许我们在服务器上将计数加倍。

首先，添加 serde 作为依赖项：

```shell
cargo add serde
```

接下来，将服务器函数添加到您的 `main.rs` 中：

```rust
{{#include src/doc_examples/server_function.rs}}
```

现在，使用 `dx build --features web` 构建您的客户端捆绑包，并使用 `cargo run --features ssr` 运行您的服务器。您应该会看到一个新按钮，它将计数乘以 2。

## 缓存数据获取

服务器函数的一个常见用例是从服务器获取数据：

```rust
{{#include src/doc_examples/server_data_fetch.rs}}
```

如果您导航到上面的站点，您将首先看到 `server data is None`，然后在 `WASM` 加载完毕并且对服务器的请求完成之后，您将看到 `server data is Some(Ok("Hello from the server!"))`。


这种方法有效，但速度可能很慢。与其等待客户端加载并向服务器发送请求，不如我们在服务器上获取页面所需的所有数据，并将这些数据与初始 HTML 页面一起发送到客户端？


这正是 `use_server_future` 钩子允许我们做的事情！`use_server_future` 类似于 `use_resource` 钩子，但它允许您在服务器上等待一个 future，并将 future 的结果发送到客户端。


让我们将数据获取更改为使用 `use_server_future`：

```rust
{{#include src/doc_examples/server_data_prefetch.rs}}
```

> 注意 `?` 在 `use_server_future` 之后。这告诉 Dioxus 全栈在继续渲染之前等待 future 解析。如果您不想等待特定的 future，可以简单地删除 ? 并手动处理 `Option`。

现在当您加载页面时，您应该会看到 `server data is Ok("Hello from the server!")`。无需等待 `WASM` 加载或等待请求完成！

```inject-dioxus
SandBoxFrame {
	url: "https://codesandbox.io/p/sandbox/dioxus-fullstack-server-future-qwpp4p?file=/src/main.rs:3,24"
}
```


## 使用 dioxus-desktop 运行客户端

到目前为止，所呈现的项目使 Web 浏览器与服务器交互，但也可以使桌面程序以类似的方式与服务器交互。（完整的示例代码可在 [Dioxus repo](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/fullstack/examples/axum-desktop) 中找到）

首先，我们需要创建两个二进制目标，一个用于桌面程序（`client.rs` 文件），一个用于服务器（`server.rs` 文件）。客户端应用程序和服务器函数在共享的 `lib.rs` 文件中编写。

桌面和服务器目标具有略微不同的构建配置，以启用额外的依赖项或功能。
完整示例中的 Cargo.toml 包含更多信息，但主要要点是：
- client.rs 必须使用 `desktop` 功能运行，以便包含可选的 `dioxus-desktop` 依赖项
- server.rs 必须使用 `ssr` 功能运行；这将生成服务器函数的服务器部分，并将运行我们的后端服务器。

创建项目后，可以使用以下命令运行服务器可执行文件：
```bash
cargo run --bin server --features ssr
```
并使用以下命令运行客户端桌面可执行文件：
```bash
cargo run --bin client --features desktop
```

### 客户端代码

客户端文件非常简单。您只需要在客户端代码中设置服务器 URL，以便它知道将网络请求发送到哪里。然后，dioxus_desktop 启动应用程序。

在开发过程中，示例项目在 `localhost:8080` 上运行服务器。**在发布之前，请务必将 URL 更新为您的生产 URL。**


### 服务器代码

在服务器代码中，首先您需要设置服务器将监听的网络地址和端口。
```rust
{{#include src/doc_examples/server_function_desktop_client.rs:server_url}}
```

然后，您需要将服务器函数宏中声明的类型注册到服务器中。
例如，考虑以下服务器函数：
```rust
{{#include src/doc_examples/server_function_desktop_client.rs:server_function}}
```

`GetServerData` 类型必须在服务器中注册，这将向服务器添加相应的路由。
```rust
{{#include src/doc_examples/server_function_desktop_client.rs:function_registration}}
```

最后，服务器启动并开始响应请求。