# TUI

您可以使用 Dioxus 构建一个在终端中运行的文本界面。

![Hello World screenshot](https://github.com/DioxusLabs/rink/raw/master/examples/example.png)

> 注意：本书是针对基于 HTML 的平台编写的。您可能能够使用 TUI 继续阅读，但需要进行一些调整。

## 支持

dioxus-tui 的开发目前暂停，我们正在专注于 Dioxus 本机渲染的基础 [Blitz](https://github.com/DioxusLabs/blitz)。将来，dioxus-tui 可能会更新以使用与 Blitz 相同的后端。如果您有兴趣为 dioxus-tui 做贡献，欢迎提交请求。

Dioxus TUI 目前处于实验阶段。但是，如果您愿意冒险进入未知领域，可以使用 `dioxus-tui` crate  构建使用有限 HTML 子集的交互式应用程序。

- 它使用 flexbox 进行布局
- 它只支持一部分属性和元素
- 常规的部件在 tui 渲染中无法工作，但 tui 渲染器有自己的部件组件，以大写字母开头。请参阅 [widgets example](https://github.com/DioxusLabs/blitz/blob/master/packages/dioxus-tui/examples/widgets.rs)
- 1px 是一个字符的行高。您常用的 CSS px 无法转换
- 如果您的应用程序发生 panic，您的终端将被破坏。这将在将来修复。