# 中间件

提取器允许你将服务器函数包装在一些代码中，这些代码可以改变请求或响应。Dioxus 全栈集成 [Tower](https://docs.rs/tower/latest/tower/index.html)，允许你将服务器函数包装在中间件中。

你可以使用 `#[middleware]` 属性向你的服务器函数添加一层中间件。让我们向服务器函数添加一个超时中间件。这个中间件将在服务器函数达到一定超时时间后停止运行：

```rust
{{#include src/doc_examples/server_function_middleware.rs:server_function_middleware}}
```