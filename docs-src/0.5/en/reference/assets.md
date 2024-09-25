# 资源

> ⚠️ 支持：Manganis 目前处于 alpha 阶段。API 更改已计划，错误更有可能发生

资源是包含在应用程序最终构建中的文件。它们可以是图像、字体、样式表或任何不是源文件的其他文件。Dioxus 提供对资源的一流支持，并提供了一种简单的方法来将它们包含在您的应用程序中，并自动为生产进行优化。

Dioxus 中的资源也与库兼容！如果您正在构建一个库，您可以在您的库中包含资源，它们将自动包含在使用您的库的任何应用程序的最终构建中。

首先，您需要将 `manganis` 箱添加到您的 `Cargo.toml` 文件中：

```sh
cargo add manganis
```

## 包含图像

要将资源包含在您的应用程序中，您只需将资源路径包装在 `mg!` 调用中。例如，要将图像包含在您的应用程序中，您可以使用以下代码：

```rust
{{#include src/doc_examples/assets.rs:images}}
```

您还可以使用 `mg!` 宏优化、调整大小和预加载图像。选择优化的文件类型（如 WebP）和合理的质量设置可以显着减小图像的大小，这有助于您的应用程序更快加载。例如，您可以使用以下代码将优化的图像包含在您的应用程序中：

```rust
{{#include src/doc_examples/assets.rs:optimized_images}}
```

## 包含任意文件

在 dioxus 桌面中，您可能希望包含一个包含应用程序数据的文件。您可以使用 `file` 函数将任意文件包含在您的应用程序中。例如，您可以使用以下代码将文件包含在您的应用程序中：

```rust
{{#include src/doc_examples/assets.rs:arbitrary_files}}
```

这些文件将自动包含在您的应用程序的最终构建中，您可以在您的应用程序中像使用任何其他文件一样使用它们。

## 包含样式表

您可以使用 `mg!` 宏将样式表包含在您的应用程序中。例如，您可以使用以下代码将样式表包含在您的应用程序中：

```rust
{{#include src/doc_examples/assets.rs:style_sheets}}
```

> [tailwind guide](../cookbook/tailwind.md) 有更多关于如何将 tailwind 与 dioxus 一起使用的信息。

## 结论

Dioxus 提供对资源的一流支持，并使将它们包含在您的应用程序中变得容易。您可以将图像、任意文件和样式表包含在您的应用程序中，dioxus 将自动为生产进行优化。这使得在您的应用程序中包含资源并确保它们针对生产进行优化变得容易。

您可以在 [manganis documentation](https://docs.rs/manganis/0.2.2/manganis/) 中阅读更多关于资源和优化资源的所有可用选项的信息。