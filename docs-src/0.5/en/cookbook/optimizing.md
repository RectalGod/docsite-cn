# 优化

*注意：本文主要针对 Web 平台，但主要的优化方法也适用于其他平台。*

你可能已经注意到，Dioxus 二进制文件相当大。
[TodoMVC app](https://github.com/tigerros/dioxus-todo-app) 的 WASM 二进制文件大小达到了 2.36mb！
别担心，我们可以将其缩减到更易于管理的 234kb。
显然，随着时间的推移，这个数字会越来越小。
使用 nightly 功能，你甚至可以将 hello world 应用程序的二进制文件大小减小到 100kb 以下！

我们还会讨论如何优化你的应用程序，以提高其速度。

但是，某些优化会以牺牲速度为代价来减小二进制文件大小，反之亦然。
你需要自己弄清楚这一点。你的应用程序是否执行性能密集型任务，例如图形处理或大量 DOM 操作？
你可以选择提高速度。不过，在大多数情况下，减小二进制文件大小是更好的选择，特别是考虑到 Dioxus WASM 二进制文件相当大。

为了测试二进制文件大小，我们将使用 [this](https://github.com/tigerros/dioxus-todo-app) 仓库作为示例应用程序。
`no-optimizations` 包将作为基础，目前大小为 2.36mb。

其他资源：
- [WASM 手册 - 缩减 `.wasm` 代码大小](https://rustwasm.github.io/docs/book/reference/code-size.html)
- [min-sized-rust](https://github.com/johnthagen/min-sized-rust)

## 以发布模式构建

这是优化项目的最佳方式。事实上，指南开头提到的 2.36mb 是在发布模式下测得的。
在调试模式下，它实际上高达 32mb！它还可以提高应用程序的速度。

幸运的是，无论你使用什么工具构建应用程序，它都可能有一个 `--release` 标记来执行此操作。

使用 [Dioxus CLI](https://dioxuslabs.com/learn/0.5/CLI) 或 [Trunk](https://trunkrs.dev/):
- Dioxus CLI: `dx build --release`
- Trunk: `trunk build --release`

## UPX

如果你不针对 Web 平台，可以使用 [UPX](https://github.com/upx/upx) CLI 工具来压缩你的可执行文件。

设置：

- 下载一个 [release](https://github.com/upx/upx/releases) 并将其解压缩到一个合适的位置。
- 将目录中的可执行文件添加到你的路径变量中。

你可以运行 `upx --help` 来获取 CLI 选项，但你还可以查看 `upx-doc.html` 以获取更详细的信息。
它包含在解压缩的目录中。

一个示例命令可能是：`upx --best -o target/release/compressed.exe target/release/your-executable.exe`。

## 构建配置

*注意：在 `.cargo/config.toml` 中定义的设置会覆盖 `Cargo.toml` 中的设置。*

除了 `--release` 标记之外，这是优化项目的最简单方法，也是最有效的方法，
至少在减小二进制文件大小方面是这样。

### 稳定

此配置 100% 稳定，并将二进制文件大小从 2.36mb 减少到 310kb。
将其添加到你的 `.cargo/config.toml`:

```toml
[profile.release]
opt-level = "z"
debug = false
lto = true
codegen-units = 1
panic = "abort"
strip = true
incremental = false
```

每个值的文档链接：
- [`opt-level`](https://doc.rust-lang.org/rustc/codegen-options/index.html#opt-level)
- [`debug`](https://doc.rust-lang.org/rustc/codegen-options/index.html#debuginfo)
- [`lto`](https://doc.rust-lang.org/rustc/codegen-options/index.html#lto)
- [`codegen-units`](https://doc.rust-lang.org/rustc/codegen-options/index.html#codegen-units)
- [`panic`](https://doc.rust-lang.org/rustc/codegen-options/index.html#panic)
- [`strip`](https://doc.rust-lang.org/rustc/codegen-options/index.html#strip)
- [`incremental`](https://doc.rust-lang.org/rustc/codegen-options/index.html#incremental)

### 不稳定

此配置包含一些不稳定的功能，但它应该可以正常工作。
它将二进制文件大小从 310kb 减少到 234kb。
将其添加到你的 `.cargo/config.toml`:

```toml
[unstable]
build-std = ["std", "panic_abort", "core", "alloc"]
build-std-features = ["panic_immediate_abort"]

[build]
rustflags = [
    "-Clto",
    "-Zvirtual-function-elimination",
    "-Zlocation-detail=none"
]

# Same as in the Stable section
[profile.release]
opt-level = "z"
debug = false
lto = true
codegen-units = 1
panic = "abort"
strip = true
incremental = false
```

*注意：每个标记中省略的空格（例如，`-C<no space here>lto`) 是故意的。这不是打字错误。*

`[profile.release]` 中的值在 [Stable](#stable) 部分有说明。每个值的文档链接：
- [`[build.rustflags]`](https://doc.rust-lang.org/cargo/reference/config.html#buildrustflags)
- [`-C lto`](https://doc.rust-lang.org/rustc/codegen-options/index.html#lto)
- [`-Z virtual-function-elimination`](https://doc.rust-lang.org/stable/unstable-book/compiler-flags/virtual-function-elimination.html)
- [`-Z location-detail`](https://doc.rust-lang.org/stable/unstable-book/compiler-flags/location-detail.html)

## wasm-opt

*注意：在未来，`wasm-opt` 将通过 [Dioxus CLI](https://crates.io/crates/dioxus-cli) 获得原生支持。*

`wasm-opt` 是 [binaryen](https://github.com/WebAssembly/binaryen) 库中的一个工具，用于优化你的 WASM 文件。
要使用它，请安装一个 [binaryen release](https://github.com/WebAssembly/binaryen/releases) 并从包目录运行以下命令：

```
wasm-opt dist/assets/dioxus/APP_NAME_bg.wasm -o dist/assets/dioxus/APP_NAME_bg.wasm -Oz
```

`-Oz` 标记指定 `wasm-opt` 应该针对大小进行优化。为了提高速度，请使用 `-O4`。

## 改善 Dioxus 代码

让我们谈谈如何改善你的 Dioxus 代码，使其性能更佳。

务必将 `rsx` 中动态部分的数量降至最低，例如条件渲染。
当 Dioxus 渲染你的组件时，它会跳过与上次渲染相同的部分。
这意味着，如果你将动态渲染降至最低，你的应用程序速度会加快，如果它不仅仅是 hello world，速度会快很多。
要查看此示例，请查看 [Dynamic Rendering](../reference/dynamic_rendering.md)。

还要查看 [Anti-patterns](antipatterns.md) 以了解你应该避免的模式。
显然，并非所有模式都与性能有关，但其中一些与性能有关。

## 优化资产的大小

资产可能是应用程序大小的重要组成部分。Dioxus 包含对第一方 [assets](../reference/assets.md) 的 Alpha 支持。你使用 `mg!` 宏包含的任何资产都将在发布版本的构建中针对生产进行优化。