# Tailwind

您可以使用您选择的任何 CSS 框架来为您的 Dioxus 应用程序设置样式，或者只编写纯 CSS。


一个流行的选项是使用 [Tailwind](https://tailwindcss.com/) 为您的 Dioxus 应用程序设置样式。Tailwind 允许您使用 CSS 实用程序类来设置元素的样式。本指南将向您展示如何在您的 Dioxus 应用程序中设置 Tailwind CSS。

## 设置

1. 安装 Dioxus CLI：

```bash
cargo install dioxus-cli
```

2. 安装 npm： [https://docs.npmjs.com/downloading-and-installing-node-js-and-npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
3. 安装 Tailwind CSS CLI： [https://tailwindcss.com/docs/installation](https://tailwindcss.com/docs/installation)
4. 初始化 Tailwind CSS 项目：

```bash
npx tailwindcss init
```

这将在项目的根目录中创建一个 `tailwind.config.js` 文件。

5. 编辑 `tailwind.config.js` 文件以包含 Rust 文件：

```js
module.exports = {
    mode: "all",
    content: [
        // include all rust, html and css files in the src directory
        "./src/**/*.{rs,html,css}",
        // include all html files in the output (dist) directory
        "./dist/**/*.html",
    ],
    theme: {
        extend: {},
    },
    plugins: [],
}
```

6. 在项目的根目录中创建一个名为 `input.css` 的文件，内容如下：

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

7. 在您的项目中添加 [Manganis](https://github.com/DioxusLabs/manganis) 以处理资产收集。

```sh
cargo add manganis
```

8. 使用 manganis 在您的 Rust 代码中的某个地方创建一个指向 `tailwind.css` 文件的链接：

```rust
{{#include src/doc_examples/tailwind.rs}}
```

### 附加步骤

1. 安装 Tailwind CSS VS Code 扩展
2. 转到扩展的设置，找到实验性正则表达式支持部分。编辑 setting.json 文件使其如下所示：

```json
"tailwindCSS.experimental.classRegex": ["class: \"(.*)\""],
"tailwindCSS.includeLanguages": {
    "rust": "html"
},
```

## 开发

- 在项目的根目录中运行以下命令以启动 Tailwind CSS 编译器：

```bash
npx tailwindcss -i ./input.css -o ./public/tailwind.css --watch
```

### Web

- 在项目的根目录中运行以下命令以启动 Dioxus 开发服务器：

```bash
dx serve
```

- 在浏览器中打开 [http://localhost:8080](http://localhost:8080)。

### 桌面

- 启动 Dioxus 桌面应用程序：

```bash
dx serve --platform desktop
```