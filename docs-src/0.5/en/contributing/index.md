# 贡献

开发在 [Dioxus GitHub repository](https://github.com/DioxusLabs/dioxus) 中进行。如果您发现错误或有功能想法，请提交问题（但首先检查是否有人 [done it already](https://github.com/DioxusLabs/dioxus/issues)）。

[GitHub discussions](https://github.com/DioxusLabs/dioxus/discussions) 可用作寻求帮助或讨论功能的地方。您也可以加入 [our Discord channel](https://discord.gg/XgGxMSkvUM)，在那里会进行一些开发讨论。

## 改善文档

如果您想改进文档，欢迎您提交 PR！Rust 文档（[source](https://github.com/DioxusLabs/dioxus/tree/main/packages)) 和本指南（[source](https://github.com/DioxusLabs/docsite/tree/main/docs-src/0.5/en)) 都可以在其各自的 GitHub 仓库中找到。

## 开发生态系统

Dioxus 的强大之处在于其丰富的生态系统。我们希望 Dioxus 也能像 React 一样拥有丰富的生态系统！所以，如果您有想编写且对许多人有益的库，我们会非常感谢。您可以 [browse npm.js](https://www.npmjs.com/search?q=keywords:react-component) 获取灵感。完成后，将您的库添加到 [awesome dioxus](https://github.com/DioxusLabs/awesome-dioxus) 列表中，或在 [Discord](https://discord.gg/XgGxMSkvUM) 的 `#I-made-a-thing` 频道中分享。

## 错误和功能

如果您已修复 [an open issue](https://github.com/DioxusLabs/dioxus/issues)，请随时提交 PR！您也可以查看 [the roadmap](./roadmap.md) 并参与其中。在开始工作之前，请考虑 [reaching out](https://discord.gg/XgGxMSkvUM) 团队，以确保每个人都在同一页面上，并且您不会做无用功！

所有拉取请求（包括团队成员提交的请求）必须至少获得另一位团队成员的批准。
关于设计、架构、重大更改、权衡等方面的较大、更微妙的决策将由团队一致决定。

## 在您贡献之前

您可能会惊讶地发现，在进行您的第一个 PR 时，许多检查会失败。
这就是为什么您应该在贡献之前先运行这些命令，这将为您节省大量时间，因为 GitHub CI 在执行所有这些命令时，要比您的 PC 慢得多。

- 使用 [rustfmt](https://github.com/rust-lang/rustfmt) 格式化代码：

```sh
cargo fmt -- src/**/**.rs
```

- 您可能需要在 Linux（Ubuntu/deb）上安装一些包，才能成功完成以下命令（仓库根目录中还有一个 Nix flake）：

```sh
sudo apt install libgdk3.0-cil libatk1.0-dev libcairo2-dev libpango1.0-dev libgdk-pixbuf2.0-dev libsoup-3.0-dev libjavascriptcoregtk-4.1-dev libwebkit2gtk-4.1-dev
```

- 检查所有代码 [cargo check](https://doc.rust-lang.org/cargo/commands/cargo-check.html)：

```sh
cargo check --workspace --examples --tests
```

- 检查 [Clippy](https://doc.rust-lang.org/clippy/) 是否会生成任何警告。请修复这些警告！

```sh
cargo clippy --workspace --examples --tests -- -D warnings
```

- 使用 [cargo-test](https://doc.rust-lang.org/cargo/commands/cargo-test.html) 测试所有代码：

```sh
cargo test --all --tests
```

- 更多测试，这次使用 [cargo-make](https://sagiegurari.github.io/cargo-make/)。以下列出了所有步骤，包括安装：

```sh
cargo install --force cargo-make
cargo make tests
```

- 使用 [MIRI](https://github.com/rust-lang/miri) 测试不安全 crate。目前，这用于 `dioxus-core` 和 `dioxus-native-core` 中的两个 MIRI 测试：

```sh
cargo miri test --package dioxus-core --test miri_stress
cargo miri test --package dioxus-native-core --test miri_native
```

- 使用 Playwright 进行测试。这将在浏览器中直接测试 UI。以下列出了所有步骤，包括安装：
  **免责声明：这可能会在您的机器上莫名其妙地失败，而并非您的错。** 尽管如此，请提交您的 PR！

```sh
cd playwright-tests
npm ci
npm install -D @playwright/test
npx playwright install --with-deps
npx playwright test
```

## 如何使用本地 crate 测试 dioxus
如果您正在开发一项功能，您应该在本地设置中对其进行测试，然后再提交 PR。此过程可确保您在同行审查代码之前了解代码的功能。

- 分叉以下 GitHub 仓库 (DioxusLabs/dioxus)：

`https://github.com/DioxusLabs/dioxus`

- 创建一个新的或使用现有的 Rust crate（如果您将使用现有的 Rust crate，请忽略此步骤）：
  我们将在此处测试分叉的库的功能

```sh
cargo new --bin demo
```

- 在 Cargo.toml 中将 dioxus 依赖项添加到您的 Rust crate（新的/现有的）：

```toml
dioxus = { path = "<path to forked dioxus project>/dioxus/packages/dioxus", features = ["web", "router"] }
```

上面的示例是针对 dioxus-web 的，包含 dioxus-router。要了解不同渲染器的依赖项，请访问 [here](https://dioxuslabs.com/learn/0.5/getting_started)。

- 运行并测试您的功能

```sh
dx serve
```

如果您是第一次使用 dioxus，请阅读 [the guide](https://dioxuslabs.com/learn/0.5/guide) 以熟悉 dioxus。