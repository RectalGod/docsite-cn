# 反模式

本示例展示了不应执行的操作，并提供了一个理由说明为什么某个模式被认为是“反模式”。大多数反模式被认为是由于性能或代码可重用性原因而错误的。

## 不必要的嵌套片段

片段不会立即将物理元素挂载到 DOM，因此 Dioxus 必须递归进入其子元素以查找物理 DOM 节点。此过程称为“规范化”。这意味着深度嵌套的片段会使 Dioxus 执行不必要的操作。在呈现真正的 DOM 元素之前，优先考虑一到两级的片段/嵌套组件。

只有组件和片段节点容易出现此问题。Dioxus 通过为组件提供注册共享状态的 API 来缓解这种情况，而无需使用 Context Provider 模式。

```rust
{{#include src/doc_examples/anti_patterns.rs:nested_fragments}}
```

## 错误的迭代器键

如 [dynamic rendering chapter](../reference/dynamic_rendering#the) 中所述，列表项必须具有唯一的键，这些键在渲染过程中与相同的项关联。这有助于 Dioxus 将状态与包含的组件关联，并确保良好的差异性能。除非您知道列表永远不会更改，否则不要省略键。

```rust
{{#include src/doc_examples/anti_patterns.rs:iter_keys}}
```

## 避免在道具中使用内部可变性

虽然在技术上可以在道具中使用 `Mutex` 或 `RwLock`，但使用它们会很困难。

假设您有一个包含字段 `username: String` 的结构体 `User`。如果您将 `Mutex<User>` 道具传递给 `UserComponent` 组件，则该组件可能希望写入 `username` 字段。但是，当它这样做时，父组件不会意识到更改，并且组件也不会重新渲染，这会导致 UI 与状态不同步。相反，考虑传递一个类似于 `Signal` 的响应式值或不可变数据。

```rust
{{#include src/doc_examples/anti_patterns.rs:interior_mutability}}
```

## 避免在渲染期间更新状态

每次更新状态时，Dioxus 都需要重新渲染组件，这效率低下！考虑重构您的代码以避免这种情况。

此外，如果您在渲染期间无条件地更新状态，它将陷入无限循环中重新渲染。

```rust
{{#include src/doc_examples/anti_patterns.rs:updating_state}}
```

## 避免大量状态组

拥有一个包含所有应用程序状态的单个大型状态结构体可能很诱人。但是，这会导致一些问题：
- 很容易意外地以导致无限循环的方式更改状态
- 难以推断何时以及如何更新状态
- 可能会导致性能问题，因为许多组件在状态更改时需要重新渲染

相反，考虑将您的状态分解为更小、更易于管理的部分。这将使您更容易推断状态，避免更新循环，并提高性能。

```rust
{{#include src/doc_examples/anti_patterns.rs:large_state}}
```

## 在组件主体中运行非确定性代码

如果您有一个包含非确定性代码的组件，该代码通常不应在组件主体中运行。如果将其放在组件主体中，它将在每次重新渲染组件时执行，这会导致性能问题。

相反，考虑将非确定性代码移到仅在组件首次创建时运行的钩子中，或者移到依赖项更改时重新运行的效果中。

```rust
{{#include src/doc_examples/anti_patterns.rs:non_deterministic}}
```

## 过于宽松的道具 PartialEq

您可能已经注意到 `Props` 需要 `PartialEq` 实现。该 `PartialEq` 对 Dioxus 正确运行非常重要。它用于确定父组件重新渲染时是否应重新渲染组件。

如果您无法为您的 `Props` 导出 `PartialEq`，则需要自行实现它。如果您确实实现了 `PartialEq`，请确保在道具以会导致子组件中的 UI 更改的方式更改时始终返回 `false`。

通常，如果您不确定道具是否已更改，则从 `PartialEq` 返回 `false` 比返回 `true` 更好。这将帮助您避免子组件中的 UI 过时。

```rust
{{#include src/doc_examples/anti_patterns.rs:permissive_partial_eq}}
```