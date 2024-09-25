# 配置项目

本章将教您如何使用 `Dioxus.toml` 文件配置 CLI。有一个 [example](#config-example)，其中包含注释以描述各个键。您可以复制它或查看此文档以获取更完整的学习体验。

"🔒" 表示必填项。某些标题是必填的，但它们内部的键都不是。在这种情况下，您只需要包含标题，而不需要包含任何键。它可能看起来很奇怪，但这是正常的。

## 结构

每个标题在其下方直接具有其 TOML 形式。

### 应用程序 🔒

```toml
[application]
```

应用程序范围的配置。适用于 Web 和桌面。

* **name** 🔒 - 项目名称和标题。
   ```toml
   name = "my_project"
   ```
* **default_platform** 🔒 - 此项目目标的平台
   ```toml
   # Currently supported platforms: web, desktop
   default_platform = "web"
   ```
* **out_dir** - 将从 `dx build` 或 `dx serve` 生成的构建工件放置的目录。这也是 `assets` 目录将被复制到的位置。
    ```toml
    out_dir = "dist"
    ```
* **asset_dir** - 具有静态资产的目录。 CLI 将在构建/服务后自动将这些资产复制到 **out_dir** 中。
   ```toml
   asset_dir = "public"
   ```
* **sub_package** - 要默认构建的工作区中的子包。
   ```toml
   sub_package = "my-crate"
   ```

### Web.App 🔒

```toml
[web.app]
```

特定于 Web 的配置。

* **title** - 网页的标题。
   ```toml
   # HTML title tag content
   title = "project_name"
   ```
* **base_path** - 用于构建应用程序以在其中提供服务的基路径。当在域下的子目录中提供应用程序时，这可能很有用。例如，当构建要在 GitHub Pages 上提供的站点时。
   ```toml
   # The application will be served at domain.com/my_application/, so we need to modify the base_path to the path where the application will be served
   base_path = "my_application"
   ```

### Web.Watcher 🔒

```toml
[web.watcher]
```

开发服务器配置。

* **reload_html** - 如果为真，CLI 将在每次重建应用程序时重建 index.html 文件
   ```toml
   reload_html = true
   ```
* **watch_path** - 要监控更改的文件和目录
   ```toml
   watch_path = ["src", "public"]
   ```

* **index_on_404** - 如果启用，Dioxus 将在找不到路由时提供根页面。
   * 这在提供使用路由器的应用程序时是必需的 *。但是，当使用 Dioxus 之外的其他东西（例如 GitHub Pages）提供您的应用程序时，您将必须检查如何在该平台上配置它。在 GitHub Pages 中，您可以将 `index.html` 的副本命名为 `404.html` 放置在同一目录中。
   ```toml
   index_on_404 = true
   ```

### Web.Resource 🔒

```toml
[web.resource]
```

静态资源配置。

* **style** - 要包含在您的应用程序中的 CSS 文件。
   ```toml
   style = [
      # Include from public_dir.
      "./assets/style.css",
      # Or some asset from online cdn.
      "https://cdn.jsdelivr.net/npm/bootstrap/dist/css/bootstrap.css"
   ]
   ```

* **script** - 要包含在您的应用程序中的 JavaScript 文件。
    ```toml
    script = [
        # Include from asset_dir.
        "./public/index.js",
        # Or from an online CDN.
        "https://cdn.jsdelivr.net/npm/bootstrap/dist/js/bootstrap.js"
    ]
   ```

### Web.Resource.Dev 🔒

```toml
[web.resource.dev]
```

这与 [`[web.resource]`](#webresource-) 相同，但它仅在开发服务器中有效。例如，如果您想在 `dx serve` 服务器中包含一个文件，但在 `dx serve --release` 服务器中不包含，请将其放在这里。

### Web.Proxy

```toml
[[web.proxy]]
```

与您的应用程序在开发期间所需的任何代理相关的配置。代理将请求转发到新的服务。

* **backend** - 要代理的服务器的 URL。CLI 将转发 backend 相对路由下的任何请求到 backend，而不是返回 404
   ```toml
   backend = "http://localhost:8000/api/"
   ```
   这将导致对带有前缀 /api/ 的开发服务器发出的任何请求被重定向到 http://localhost:8000 的后端服务器。路径和查询参数将按原样传递（目前不支持路径重写）。

## 配置示例

这包括所有字段，无论是否必填。

```toml
[application]

# App name
name = "project_name"

# The Dioxus platform to default to
default_platform = "web"

# `build` & `serve` output path
out_dir = "dist"

# The static resource path
asset_dir = "public"

[web.app]

# HTML title tag content
title = "project_name"

[web.watcher]

# When watcher is triggered, regenerate the `index.html`
reload_html = true

# Which files or dirs will be monitored
watch_path = ["src", "public"]

# Include style or script assets
[web.resource]

# CSS style file
style = []

# Javascript code file
script = []

[web.resource.dev]

# Same as [web.resource], but for development servers

# CSS style file
style = []

# JavaScript files
script = []

[[web.proxy]]
backend = "http://localhost:8000/api/"
```