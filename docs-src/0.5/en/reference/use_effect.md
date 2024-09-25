# 使用 Effect

`use_effect`(https://docs.rs/dioxus-hooks/latest/dioxus_hooks/fn.use_effect.html) 允许你在当前渲染结束后运行回调函数，当其依赖项发生变化时会重新运行。Effect 是处理副作用（例如手动更改 DOM）的一种灵活方式。

如果你正在寻找一个带有依赖项的异步任务处理钩子，你应该使用 `use_resource`。 或者，如果你想从带有依赖项的回调中生成一个新值，你应该使用 `use_memo`。

## 依赖项

你可以让回调函数在某些值发生变化时重新运行。例如，你可能希望仅在用户 ID 发生变化时才获取用户数据。Effect 会自动订阅你在 effect 内部读取的所有信号。它会在任何这些信号发生变化时重新运行。

## 示例

```rust, no_run
{{#include src/doc_examples/use_effect.rs}}
```