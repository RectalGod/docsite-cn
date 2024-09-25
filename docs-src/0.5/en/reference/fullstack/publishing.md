## 使用 Github Pages 发布

您可以使用 Dioxus 全栈来预渲染您的应用程序，然后在客户端进行水合。这非常适合在像 Github Pages 这样的提供商上静态托管的页面。实际上，官方 Dioxus 网站使用了这种方法。

您可以将您的应用程序设置为静态生成所有静态页面：

```rust
{{#include src/doc_examples/fullstack_static.rs}}
```

接下来，编辑您的 `Dioxus.toml`，将您的 `out_dir` 指向 `docs` 文件夹，并将 `base_path` 设置为您的仓库名称：

```toml
[application]
# ...
out_dir = "docs"

[web.app]
base_path = "your_repo"
```

然后构建您的应用程序并发布到 Github：

- 确保为您的仓库设置了 GitHub Pages，以便将 docs 目录中的所有静态文件发布。
- 使用以下命令构建您的应用程序：
```sh
dx build --release --features web
cargo run --features ssr
```
- 使用 git 添加并提交更改。
- 推送到 GitHub。