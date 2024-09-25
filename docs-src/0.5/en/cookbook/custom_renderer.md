# 自定义渲染器

Dioxus 是一个极其便携的 UI 开发框架。随着时间的推移，你所获得的经验、知识、钩子和组件始终可以用于未来的项目。然而，有时这些项目无法利用支持的渲染器，或者你需要实现自己的更好的渲染器。

好消息是：渲染器的设计完全由你决定！我们通过第一方渲染器提供建议和灵感，但实际上只需要处理 `Mutations` 并发送 `UserEvents`。

## 具体内容：

实现渲染器相当简单。渲染器需要：

1. 处理由虚拟 DOM 更新生成的编辑流。
2. 注册监听器并将事件传递到虚拟 DOM 的事件系统中。

本质上，你的渲染器需要处理编辑并生成事件来更新 VirtualDOM。从那里，你将拥有渲染 VirtualDOM 到屏幕上所需的一切。

在内部，Dioxus 处理树关系、差异化、内存管理和事件系统，尽可能减少渲染器需要自己实现的功能。

作为参考，可以查看 [javascript interpreter](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/interpreter) 或 [tui renderer](https://github.com/DioxusLabs/blitz/tree/master/packages/dioxus-tui) 作为你自定义渲染器的起点。

## 模板

Dioxus 是围绕 [Templates](https://docs.rs/dioxus-core/latest/dioxus_core/prelude/struct.Template.html) 的概念构建的。模板描述了在编译时已知的 UI 树，其中动态部分在运行时填充。这在内部很有用，可以使跳过差异化静态节点，但它也对渲染器重用 UI 树的部分很有用。这对于诸如项目列表之类的东西很有用。每个项目都可以包含一些静态部分和一些动态部分。渲染器可以使用模板来创建 UI 的静态部分一次，将其克隆到列表中的每个元素，然后填充动态部分。

## 变异

`Mutation` 类型是一个序列化枚举，它表示应该应用于更新 UI 的操作。变体大致遵循以下集合：

```rust
enum Mutation {
	AppendChildren,
	AssignId,
	CreatePlaceholder,
	CreateTextNode,
	HydrateText,
	LoadTemplate,
	ReplaceWith,
	ReplacePlaceholder,
	InsertAfter,
	InsertBefore,
	SetAttribute,
	SetText,
	NewEventListener,
	RemoveEventListener,
	Remove,
	PushRoot,
}
```

Dioxus 的差异化机制作为 [stack machine](https://en.wikipedia.org/wiki/Stack_machine) 工作，其中 [LoadTemplate](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.LoadTemplate), [CreatePlaceholder](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.CreatePlaceholder), 和 [CreateTextNode](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.CreateTextNode) 变异将新的“真实”DOM 节点推送到堆栈上，[AppendChildren](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.AppendChildren), [InsertAfter](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.InsertAfter), [InsertBefore](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.InsertBefore), [ReplacePlaceholder](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.ReplacePlaceholder), 和 [ReplaceWith](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.ReplaceWith) 都从堆栈中删除节点。

## 节点存储

Dioxus 使用 ID 保存和加载元素。在 VirtualDOM 中，这只是作为 u64 跟踪的。

每当在差异化期间生成 `CreateElement` 编辑时，Dioxus 都会增加其节点计数器并将该新元素分配给它当前的 NodeCount。RealDom 负责记住此 ID 并在变异中使用 ID 时推送正确的节点。Dioxus 在元素被删除时回收元素的 ID。为了与 Dioxus 保持同步，你可以使用稀疏 Vec (Vec<Option<T>>)，其中可能包含未占用的项。你可以使用 ID 作为 Vec 中元素的索引，并在 ID 不存在时扩展 Vec。

### 示例

为了理解，让我们考虑这个示例 - 一个非常简单的 UI 声明：

```rust
rsx! { h1 { "count: {x}" } }
```

#### 构建模板

上面的 rsx 将创建一个包含一个静态 h1 标签和一个动态文本节点占位符的模板。该模板包含 UI 的静态部分以及动态部分的 ID 以及访问它们的路径。

该模板看起来类似于：

```rust
Template {
	// Some id that is unique for the entire project
	name: "main.rs:1:1:0",
	// The root nodes of the template
	roots: &[
		TemplateNode::Element {
			tag: "h1",
			namespace: None,
			attrs: &[],
			children: &[
				TemplateNode::DynamicText {
					id: 0
				},
			],
		}
	],
	// the path to each of the dynamic nodes
	node_paths: &[
		// the path to dynamic node with a id of 0
		&[
			// on the first root node
			0,
			// the first child of the root node
			0,
		]
	],
	// the path to each of the dynamic attributes
	attr_paths: &'a [&'a [u8]],
}
```

> 有关模板结构的更多详细信息，请参阅 [Template api docs](https://docs.rs/dioxus-core/latest/dioxus_core/prelude/struct.Template.html)

该模板将在 [list of templates](https://docs.rs/dioxus-core/latest/dioxus_core/struct.Mutations.html#structfield.templates) 中发送到渲染器，其中包含它第一次使用时的变异。渲染器在遇到 [LoadTemplate](https://docs.rs/dioxus-core/latest/dioxus_core/enum.Mutation.html#variant.LoadTemplate) 变异后，它应该克隆该模板并将其存储在给定的 ID 中。

对于动态节点和动态文本节点，应该创建一个占位符节点并将其插入 UI 中，以便稍后可以修改该节点。

在 HTML 渲染器中，该模板可能如下所示：

```html
<h1>""</h1>
```

#### 应用变异

渲染器创建完所有新模板后，就可以开始处理变异了。

渲染器启动时，应该包含堆栈上的 Root 节点并将 Root 节点存储为 ID 为 0。Root 节点是 UI 的顶级节点。在 HTML 中，这是 `<div id="main">` 元素。

```rust
instructions: []
stack: [
	RootNode,
]
nodes: [
	RootNode,
]
```

第一个变异是 `LoadTemplate` 变异。这告诉渲染器从具有给定 ID 的模板中加载根。然后渲染器会将模板的根节点推送到堆栈上，并将其存储为 ID 以备后用。在这种情况下，根节点是 h1 元素。

```rust
instructions: [
	LoadTemplate {
		// the id of the template
		name: "main.rs:1:1:0",
		// the index of the root node in the template
		index: 0,
		// the id to store
		id: ElementId(1),
	}
]
stack: [
	RootNode,
	<h1>""</h1>,
]
nodes: [
	RootNode,
	<h1>""</h1>,
]
```

接下来，Dioxus 将创建动态文本节点。差异化算法决定需要创建此节点，因此 Dioxus 将生成变异 `HydrateText`。当渲染器收到此指令时，它将导航到模板中的占位符文本节点，并将其替换为新文本。

```rust
instructions: [
	LoadTemplate {
		name: "main.rs:1:1:0",
		index: 0,
		id: ElementId(1),
	},
	HydrateText {
		// the id to store the text node
		id: ElementId(2),
		// the text to set
		text: "count: 0",
	}
]
stack: [
	RootNode,
	<h1>"count: 0"</h1>,
]
nodes: [
	RootNode,
	<h1>"count: 0"</h1>,
	"count: 0",
]
```

请记住，h1 节点没有附加到任何内容（它已卸载），因此 Dioxus 需要生成将 h1 节点连接到 Root 的 Edit。这取决于情况，但在这种情况下，我们使用 `AppendChildren`。这会将文本节点从堆栈中弹出，将 Root 元素作为堆栈上的下一个元素留下。

```rust
instructions: [
	LoadTemplate {
		name: "main.rs:1:1:0",
		index: 0,
		id: ElementId(1),
	},
	HydrateText {
		id: ElementId(2),
		text: "count: 0",
	},
	AppendChildren {
		// the id of the parent node
		id: ElementId(0),
		// the number of nodes to pop off the stack and append
		m: 1
	}
]
stack: [
	RootNode,
]
nodes: [
	RootNode,
	<h1>"count: 0"</h1>,
	"count: 0",
]
```

随着时间的推移，我们的堆栈看起来像这样：

```rust
[Root]
[Root, <h1>""</h1>]
[Root, <h1>"count: 0"</h1>]
[Root]
```

方便的是，这种方法完全分离了 Virtual DOM 和 Real DOM。此外，这些编辑是可序列化的，这意味着我们甚至可以跨网络连接管理 UI。这种小型堆栈机和序列化编辑使 Dioxus 独立于平台细节。

Dioxus 也非常快。由于 Dioxus 将差异化和修补阶段分开，因此它能够在很短的时间内（不到一帧）对 RealDOM 进行所有编辑，从而使渲染非常流畅。它还允许 Dioxus 在差异化期间如果出现更高优先级的任务，则取消大型差异化操作。

这个小演示是为了展示渲染器如何需要处理变异流以构建 UI。

## 事件循环

与大多数 GUI 一样，Dioxus 依赖于事件循环来推进 VirtualDOM。VirtualDOM 本身也可以产生事件，因此你的自定义渲染器能够处理这些事件也很重要。

WebSys 实现的代码很简单，因此我们将将其添加到这里以演示事件循环是多么简单：

```rust, ignore
pub async fn run(&mut self) -> dioxus_core::error::Result<()> {
	// Push the body element onto the WebsysDom's stack machine
	let mut websys_dom = crate::new::WebsysDom::new(prepare_websys_dom());
	websys_dom.stack.push(root_node);

	// Rebuild or hydrate the virtualdom
	let mutations = self.internal_dom.rebuild();
	websys_dom.apply_mutations(mutations);

	// Wait for updates from the real dom and progress the virtual dom
	loop {
		let user_input_future = websys_dom.wait_for_event();
		let internal_event_future = self.internal_dom.wait_for_work();

		match select(user_input_future, internal_event_future).await {
			Either::Left((_, _)) => {
				let mutations = self.internal_dom.work_with_deadline(|| false);
				websys_dom.apply_mutations(mutations);
			},
			Either::Right((event, _)) => websys_dom.handle_event(event),
		}

		// render
	}
}
[[[CODE_BLOCK_a4340f81-3973-4464-94b8-5c6cee78dd5d]]]rust, ignore
fn virtual_event_from_websys_event(event: &web_sys::Event) -> VirtualEvent {
	match event.type_().as_str() {
		"keydown" => {
			let event: web_sys::KeyboardEvent = event.clone().dyn_into().unwrap();
			UserEvent::KeyboardEvent(UserEvent {
				scope_id: None,
				priority: EventPriority::Medium,
				name: "keydown",
				// This should be whatever element is focused
				element: Some(ElementId(0)),
				data: Arc::new(KeyboardData{
					char_code: event.char_code(),
					key: event.key(),
					key_code: event.key_code(),
					alt_key: event.alt_key(),
					ctrl_key: event.ctrl_key(),
					meta_key: event.meta_key(),
					shift_key: event.shift_key(),
					location: event.location(),
					repeat: event.repeat(),
					which: event.which(),
				})
			})
		}
		_ => todo!()
	}
}
[[[CODE_BLOCK_e71635bc-2596-4773-a63a-9d412a6308c8]]]rust
rsx!{
	div{
		color: "red",
		p{
			border: "1px solid black",
			"hello world"
		}
	}
}
[[[CODE_BLOCK_25cb45d0-b4de-4615-929f-0973fd0ce1f0]]]rust, ignore
{{#include src/doc_examples/custom_renderer.rs:derive_state}}
[[[CODE_BLOCK_41764c4c-106b-400f-a567-246af2346d25]]]rust
{{#include src/doc_examples/custom_renderer.rs:state_impl}}
[[[CODE_BLOCK_e0beb0d7-317b-4ecf-bcfd-49219b4b95b9]]]rust
{{#include src/doc_examples/custom_renderer.rs:rendering}}
[[[CODE_BLOCK_b1b20f41-5042-4b29-b191-2d7219b9bbb9]]]rust
{{#include src/doc_examples/custom_renderer.rs:cursor}}
```

## 结论

就这样！你应该拥有实现渲染器所需的大部分知识。我们非常乐意看到 Dioxus 应用被带到自定义桌面渲染器、移动渲染器、视频游戏 UI 甚至增强现实中！如果你有兴趣为其中任何一个项目做出贡献，不要害怕与我们联系或加入 [community](https://discord.gg/XgGxMSkvUM)。