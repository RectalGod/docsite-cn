# 处理外部状态

本指南将帮助您将 Dioxus 应用程序与一些外部状态集成，例如不同的线程或 WebSocket 连接。

## 处理非响应式状态

[Coroutines](../../reference/use_coroutine.md) 是处理应用程序中非响应式（您不直接渲染的状态）状态的绝佳工具。


您可以在协程异步块中存储状态，并使用来自任何子组件的消息与协程进行通信。

```rust
{{#include src/doc_examples/use_coroutine.rs:use_coroutine}}
```

## 使响应式状态成为外部状态

如果您有一些响应式状态（渲染的状态），想要从另一个线程修改它，可以使用同步信号。信号可以可选地用一个第二泛型值来表示同步信息。同步信号比线程本地信号开销略高，但它们可以在多线程环境中使用。

```rust
{{#include src/doc_examples/sync_signal.rs}}
```