# 入门

本节将帮助您设置 Dioxus 项目！

## 先决条件

### 编辑器

Dioxus 与 [Rust-Analyzer LSP plugin](https://rust-analyzer.github.io) 集成得很好，它将提供适当的语法高亮、代码导航、折叠等功能。

### Rust

前往 [https://rust-lang.org](http://rust-lang.org) 并安装 Rust 编译器。

我们强烈建议您完整地阅读 [official Rust book](https://doc.rust-lang.org/book/ch01-00-getting-started.html)。但是，我们希望 Dioxus 应用可以作为您第一个 Rust 项目的良好选择。使用 Dioxus，您将学习以下内容：

- 错误处理
- 结构体、函数、枚举
- 闭包
- 宏

我们非常注重使 Dioxus 语法易于理解和熟悉，因此您不必在开始构建复杂的 Dioxus 应用之前就深入了解异步、生命周期或智能指针。

### 平台特定依赖项

大多数平台不需要任何其他依赖项，但如果您要针对桌面平台，可以安装以下依赖项：

```inject-dioxus
DesktopDependencies {}
```

### Dioxus CLI

接下来，让我们安装 Dioxus CLI：

```
cargo install dioxus-cli
```

如果您在安装时遇到 OpenSSL 错误，请确保安装了 [here](https://docs.rs/openssl/latest/openssl/#automatic) 中列出的依赖项。

## 创建一个新项目

您可以通过运行以下命令并按照提示操作来创建一个新的 Dioxus 项目：

```sh
dx new
```

```inject-dioxus
video {
    "type": "video/mp4",
    "name": "dx new demo",
    autoplay: "true",
    controls: "false",
    r#loop: "true",
    width: "800px",
    muted: "true",
    source {
        src: manganis::mg!(file("./public/static/dioxus-new.mov")),
    }
}
```

首先，您需要选择一个平台。每个平台都有自己的参考，其中包含有关如何为该平台设置项目的更多信息。以下是我们建议您从哪些平台开始：

- Web
    - [Client Side](../reference/web/index.md): 通过 WebAssembly 在浏览器中运行
    - [Fullstack](../reference/fullstack/index.md): 在服务器上渲染为 HTML 文本，并在客户端对其进行水合
> 如果您不确定要使用哪个 Web 平台，请查看 [choosing a web renderer](choosing_a_web_renderer.md) 章节。
- WebView
    - [Desktop](../reference/desktop/index.md): 在桌面上的 Web 视图中运行
    - [Mobile](../reference/mobile/index.md): 在移动设备上的 Web 视图中运行。目前，移动设备不受 dioxus CLI 支持。[mobile reference](../reference/mobile/index.md) 中有关于设置移动项目的更多信息

接下来，您可以选择一个样式库。对于这个项目，我们将使用纯 CSS。

最后，您可以选择在启用路由器的情况下启动项目。路由器在 [router guide](../router/index.md) 中有介绍。

## 运行项目

创建项目后，您可以使用以下命令启动它：

```sh
cd my_project
dx serve
```

对于使用 liveview 模板的项目，请运行 `dx serve --desktop`。

对于 Web 目标，应用程序将提供服务于 [http://localhost:8080](http://localhost:8080)

## 结论

就是这样！您现在已经拥有了一个可用的 Dioxus 项目。您可以继续学习 Dioxus，方法是 [making a hackernews clone in the guide](../guide/index.md)，或在 [reference](../reference/index.md) 中学习特定主题/平台。如果您有任何问题，请随时在 [discord](https://discord.gg/XgGxMSkvUM) 或 [open a discussion](https://github.com/DioxusLabs/dioxus/discussions) 中提问。