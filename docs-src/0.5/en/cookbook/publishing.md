# 发布

在构建完应用后，你需要将其发布到某个地方。本参考将概述发布桌面或 Web 应用的不同方法。

## Web：使用 GitHub Pages 发布

编辑你的 `Dioxus.toml`  以将你的 `out_dir` 指向 `docs` 文件夹，并将 `base_path` 设置为你的仓库名称：

```toml
[application]
# ...
out_dir = "docs"

[web.app]
base_path = "your_repo"
```

然后构建你的应用并将其发布到 GitHub：

- 确保 GitHub Pages 已为你的仓库设置好，以便发布 docs 目录中的所有静态文件
- 使用以下命令构建你的应用：
```sh
dx build --release
```
- 复制你的 `docs/index.html` 文件，并将副本重命名为 `docs/404.html`，以便你的应用可以与客户端路由一起工作
- 使用 git 添加并提交
- 推送到 GitHub

## 桌面：创建安装程序

Dioxus 桌面应用使用你操作系统的 WebView 库，因此它可以移植到其他平台上进行分发。

在本节中，我们将介绍如何将你的应用打包到 macOS、Windows 和 Linux。

## 准备你的应用以进行打包

根据你的平台，你可能需要在你的 `main.rs` 文件中添加一些额外的代码，以确保你的应用已准备好进行打包。在 Windows 上，你需要在你的 `main.rs` 文件中添加 `#![windows_subsystem = "windows"]` 属性，以隐藏运行应用时弹出的终端窗口。**如果你在 Windows 上开发，仅在打包时使用此方法。** 这将禁用终端，因此你将无法获得任何日志。你可以将其置于某个特性后面，如下所示：

```toml
# Cargo.toml
[features]
bundle = []
```

然后你的 `main.rs`:

```rust
#![cfg_attr(feature = "bundle", windows_subsystem = "windows")]
```

## 将资源添加到你的应用中

如果你想将资源打包到你的应用中，你可以使用 `manganis` crate（在 [assets](../reference/assets.md) 页面中详细介绍），或者将它们包含在你的 `Dioxus.toml` 文件中：

```toml
[bundle]
# The list of files to include in the bundle. These can contain globs.
resources = ["main.css", "header.svg", "**/*.png"]
```

## 安装 `dioxus CLI`

我们要做的第一件事是安装 [dioxus-cli](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/cli)。此 cargo 扩展将使我们非常轻松地为各种平台打包我们的应用。

要安装，只需运行：

`cargo install dioxus-cli`

## 构建

要打包你的应用，你可以简单地运行 `dx bundle --release`（如果你在使用，还需要添加 `--features bundle`，请参阅 [this](#preparing-your-application-for-bundling) 获取更多信息），以生成一个包含所有优化和资源的最终应用。

运行完该命令后，你的应用应该可以在 `dist/bundle/` 中访问。

例如，macOS 应用将如下所示：

![Published App](public/static/publish.png)

很好！它只有 4.8 MB – 非常精简！由于 Dioxus 利用了你的平台的原生 WebView，Dioxus 应用非常节省内存，不会浪费你的电池。

> 注意：并非所有 CSS 在所有平台上都以相同的方式工作。请确保在发布之前查看你的应用的 CSS 在每个平台上 – 或 Web 浏览器（Firefox、Chrome、Safari）上的显示效果。