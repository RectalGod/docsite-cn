# 创建项目

安装完 Dioxus CLI 后，你可以用它来创建自己的项目！

## 初始化项目

首先，运行 `dx new` 命令来创建一个新项目。

> 它会克隆这个 [template](https://github.com/DioxusLabs/dioxus-template)，用于创建 Dioxus 应用程序。
>
> 你可以通过传递 `template` 参数，从不同的模板创建项目：
> ```
> dx new --template gh:dioxuslabs/dioxus-template
> ```

接下来，使用 `cd project-name` 进入你的新项目，或者直接在 IDE 中打开它。

> 确保在运行项目之前安装了 WASM 目标。
> 你可以使用 rustup 安装 WASM 目标：
> ```
> rustup target add wasm32-unknown-unknown
> ```

最后，使用 `dx serve` 启动你的项目！CLI 会告诉你它正在服务的地址，以及代码警告等其他信息。