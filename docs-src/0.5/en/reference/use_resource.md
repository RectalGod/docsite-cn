# 资源

`use_resource`(https://docs.rs/dioxus-hooks/latest/dioxus_hooks/fn.use_resource.html) 允许您运行异步闭包并提供其结果。

例如，我们可以在 `use_resource` 内部进行 API 请求（使用 [reqwest](https://docs.rs/reqwest/latest/reqwest/index.html)）：

```rust
{{#include src/doc_examples/use_resource.rs:use_resource}}
```

`use_resource` 内部的代码将在组件渲染完成后提交到 Dioxus 调度程序。

我们可以使用 `.read()` 获取未来结果。在第一次运行时，由于组件加载时没有准备好的数据，它的值将是 `None`。但是，一旦未来完成，组件将重新渲染，并且该值现在将是 `Some(...)`，其中包含闭包的返回值。

然后我们可以渲染结果：

```rust
{{#include src/doc_examples/use_resource.rs:render}}
```

```inject-dioxus
DemoFrame {
    use_resource::App {}
}
```

## 重启未来

`Resource` 句柄提供了一个 `restart` 方法。它可以用来再次执行未来，产生一个新的值。

## 依赖关系

通常，您需要在每次某些值（例如状态）发生变化时再次运行未来。与其手动调用 `restart`，您可以在未来内读取一个信号。当您在未来读取的任何状态发生变化时，它将自动重新运行未来。示例：

```rust, no_run
{{#include src/doc_examples/use_resource.rs:dependency}}
```