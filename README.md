由免费的 [TranxGod](https://github.com/RectalGod/TranxGod) 项目提供翻译支持

# dioxuslabs.com

此仓库包含 https://dioxuslabs.com 网站的源代码。

该网站使用 Dioxus 编写，由 `dioxus_ssr` 预生成，然后通过 `dioxus_web` 提供的交互性进行重新水化。

## 开发

在项目的根目录中运行以下命令以启动 Tailwind CSS 编译器：

```bash
npx tailwindcss -i ./tailwind.css -o ./public/tailwind.css --watch
```

可以使用任何文本编辑器编辑文档。大多数常用的编辑器都支持 `markdown` 格式的语法高亮。要查看更改，可以安装 [`dx`][dx] 工具到本地，前提是您已经拥有一个可用的 `Rust` 设置：

```sh
cargo install dioxus-cli
```

安装 [`dx`][dx] 后，您可以使用它在本地系统上构建和提供文档：

```sh
dx serve
```

这将启动一个本地服务器，该服务器将在 [localhost:8080](localhost:8080) 上可用，并将自动构建和重新构建文档，以响应更改。

## 贡献

- 查看网站上的 [贡献部分]
- 在我们的 [问题追踪器] 上报告问题
- 加入 Discord 并提出问题！

<a href="https://github.com/dioxuslabs/docsite/graphs/contributors">
  <img
    src="https://contrib.rocks/image?repo=dioxuslabs/docsite&max=30&columns=10"
  />
</a>

[dx]: https://github.com/DioxusLabs/dioxus/tree/main/packages/cli
[贡献部分]: https://dioxuslabs.com/learn/0.5/contributing
[问题追踪器]: https://github.com/dioxuslabs/docsite/issues
