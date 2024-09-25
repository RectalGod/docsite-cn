# 协程

协程是异步工具箱中的另一个工具。协程是能够接收值的未来对象。

与普通未来对象一样，协程中的代码将在遇到下一个`await`点之前运行。这种对异步任务的低级控制非常强大，允许实现无限循环的任务，例如 WebSocket 轮询、后台计时器和其他周期性操作。

## `use_coroutine`

`use_coroutine` 钩子允许您创建协程。我们编写的多数协程将是使用 await 进行轮询循环的。

```rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:component}}
[[[CODE_BLOCK_5dc4687f-c020-4d80-988a-e278aed14e4f]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:to_owned}}
[[[CODE_BLOCK_d0999ecc-eb56-4763-8c33-6bb5b45faaf7]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:to_owned_continued}}
[[[CODE_BLOCK_8a9997a9-8796-4960-8c2a-2adabc935655]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:send}}
[[[CODE_BLOCK_e9c42513-e90a-48de-a5e7-23447daa1f03]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:services}}
[[[CODE_BLOCK_c34c4ce4-1501-44fb-b3eb-97c23476f501]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:global}}
[[[CODE_BLOCK_9fb7d05b-bdc9-4a93-8668-0fbebf057f77]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:global_continued}}
[[[CODE_BLOCK_b7e4ee8f-cae9-40af-b813-516c1ec39323]]]rust, no_run
{{#include src/doc_examples/use_coroutine_reference.rs:injection}}
```