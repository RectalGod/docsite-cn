# Hello World 示例内部机制详解

本指南将带您深入了解 Hello World 示例程序的内部机制。它将解释 Dioxus 内部机制的主要部分如何相互作用，将 README 示例从源文件转化为正在运行的应用程序。本指南旨在提供 Dioxus 内部机制的高级概述，并非详尽无遗的指南。

Dioxus 的核心库大致按以下方式运作：

![](https://mermaid.ink/img/pako:eNqNk01v2zAMhv8KocsuTQ876lCgWAb0sGDD0mMAg7PoWogsBvpwWhT976MlJ3OKbKtOEvmIfEWRr6plQ0qrmDDR2uJTwGE1ft55kBXIGwqNHQYyVvywWt3BA3rjKGj4gs5BX0-V_1n4QtUthW_Mh6WzWgryg537OpJPsQJ_zsX9PrmG0fBwWxM2NIH1nmdRFuxTn4C7K4mn9djTpYAjWsnTcQBaSJiWxIcULEVILCIiu5Egyf3RhpTRwfr75tOC73LKggGmQkUcBLcDVUJyFoF_qcEkoxEVzZHDvjIXpnOhtm1PJp8rvcGw37Z8oPu4FlkvhVvbrivGypyP_3dWXRo2WdrAsp-fN391Qd5n1BBnSU0-GDy9sHyGo678xcOyOU7fMHcMHINNtcgIPfP-Wr2WAu6NeeRzGTS0z7fxgEd_7T3_Zi8b5kp1T1IxvvgWfjlu9x-SexHqo1VTN2qgMKA1MoavU6CdkkaSBlJatoY6zC7t1M6_CYo58VZUKZ1CphtVo8yDq3SHLopVJiZx2NTRLhP-9htxEk8q?type=png)

## 源文件

我们从一个 Hello World 程序开始。该程序渲染一个桌面应用程序，并在网页视图中显示文本 "Hello World"。

```rust, no_run
{{#include src/doc_examples/readme.rs}}
[[[CODE_BLOCK_38d31399-4f79-4cc3-9ffb-c84f3064d26f]]]rust, no_run
{{#include src/doc_examples/readme_expanded.rs}}
[[[CODE_BLOCK_1206a7c7-32a7-4282-8485-7306e62a895a]]]rust, no_run
pub struct VirtualDom {
    // All the templates that have been created or set during hot reloading
    pub(crate) templates: FxHashMap<TemplateId, FxHashMap<usize, Template<'static>>>,

    // A slab of all the scopes that have been created
    pub(crate) scopes: ScopeSlab,

    // All scopes that have been marked as dirty
    pub(crate) dirty_scopes: BTreeSet<DirtyScope>,

    // Every element is actually a dual reference - one to the template and the other to the dynamic node in that template
    pub(crate) elements: Slab<ElementRef>,

    // This receiver is used to receive messages from hooks about what scopes need to be marked as dirty
    pub(crate) rx: futures_channel::mpsc::UnboundedReceiver<SchedulerMsg>,

    // The changes queued up to be sent to the renderer
    pub(crate) mutations: Mutations<'static>,
}
```

> 什么是 [slab](https://docs.rs/slab/latest/slab/)?
>
> 如果你不关心键的值，Slab 就像一个具有整型键的哈希表。它在内部由一个密集的向量支持，使其比哈希表更高效。当你将一个值插入 Slab 时，它会返回一个整型键，你可以用它来在以后检索该值。

> Dioxus 如何使用 Slab？
>
> Dioxus 使用“同步 Slab”在渲染器和 VDOM 之间进行通信。当在虚拟 DOM 中创建一个节点时，一个 (elementId, mutation) 对被传递给渲染器以标识该节点，渲染器随后会将该节点渲染到实际 DOM 中。这些 ID 也被虚拟 DOM 用于在未来的突变中引用该节点，例如设置节点的属性或移除节点。当渲染器向虚拟 DOM 发送事件时，它会发送触发该事件的节点的 ElementId。虚拟 DOM 使用此 ID 在 Slab 中找到该节点，然后运行必要的事件处理程序。

虚拟 DOM 是一个作用域树。每个组件在首次渲染时都会创建一个新的 `Scope`，并在组件卸载时回收。

作用域主要有三个目的：

1. 它们存储组件使用的钩子的状态。
2. 它们存储上下文 API 的状态（例如：使用
   [use_context_provider](https://docs.rs/dioxus/latest/dioxus/prelude/fn.use_context_provider.html)）。
3. 它们存储渲染的 `VNode` 的当前版本和先前版本，以便可以
   进行差异比较以生成重新渲染所需的突变集。

### 初始渲染

根作用域被创建和重建：

1. 运行根组件。
2. 根组件返回一个 `VNode`。
3. 为此 `VNode` 创建突变并将其添加到突变列表中（这可能涉及创建新的子组件）。
4. `VNode` 被存储在根的 `Scope` 中。

在根的 `Scope` 被构建后，所有生成的突变都被发送到渲染器，渲染器将它们应用于 DOM。

在初始渲染之后，根 `Scope` 如下所示：

[![](https://mermaid.ink/img/pako:eNqtVE1P4zAQ_SuzPrWikRpWXCLtBRDisItWsOxhCaqM7RKricdyJrQV8N93QtvQNCkfEnOynydv3nxkHoVCbUQipjnOVSYDwc_L1AFbWd3dB-kzuEQkuFLoDUwDFkCZAek9nGDh0RlHK__atA1GkUUHf45f0YbppAqB_aOzIAvz-t7-chN_Y-1bw1WSJKsglIu2w9tktWXxIIuHURT5XCqTYa5NmDguw2R8c5MKq2GcgF46WTB_jafi9rZL0yi5q4jQTSrf9altO4okCn1Ratwyz55Qxuku2ITlTMgs6HCQimsPmb3PvqVi-L5gjXP3QcnxWnL8JZLrwGvR31n0KV-Bx6-r-oVkT_-3G1S-NQLbk9i8rj7udP2cixed2QcDCitHJiQw7ub3EVlNecrPjudG2-6soFO5VbMECmR9T5OnlUY4-AFxfw9aTFst3McU9TK1Otm6NEn_DubBYlX2_dglLXOz48FgwJmJ5lZTlhz6xWgNaFnyDgpymcARHO0W2a9J_l5w2wYXvHuGPcqaQ-rESBQmFNJq3nCPNZoK3l4sUSR81DLMUpG6Z_aTFeHV0imRUKjMSFReSzKnVnKGhUimMi8ZNdoShl-rlfmyOUfCS_cPcePz_B_Wl4pc?type=png)](https://mermaid.live/edit#pako:eNqtVE1P4zAQ_SuzPrWikRpWXCLtBRDisItWsOxhCaqM7RKricdyJrQV8N93QtvQNCkfEnOynydv3nxkHoVCbUQipjnOVSYDwc_L1AFbWd3dB-kzuEQkuFLoDUwDFkCZAek9nGDh0RlHK__atA1GkUUHf45f0YbppAqB_aOzIAvz-t7-chN_Y-1bw1WSJKsglIu2w9tktWXxIIuHURT5XCqTYa5NmDguw2R8c5MKq2GcgF46WTB_jafi9rZL0yi5q4jQTSrf9altO4okCn1Ratwyz55Qxuku2ITlTMgs6HCQimsPmb3PvqVi-L5gjXP3QcnxWnL8JZLrwGvR31n0KV-Bx6-r-oVkT_-3G1S-NQLbk9i8rj7udP2cixed2QcDCitHJiQw7ub3EVlNecrPjudG2-6soFO5VbMECmR9T5OnlUY4-AFxfw9aTFst3McU9TK1Otm6NEn_DubBYlX2_dglLXOz48FgwJmJ5lZTlhz6xWgNaFnyDgpymcARHO0W2a9J_l5w2wYXvHuGPcqaQ-rESBQmFNJq3nCPNZoK3l4sUSR81DLMUpG6Z_aTFeHV0imRUKjMSFReSzKnVnKGhUimMi8ZNdoShl-rlfmyOUfCS_cPcePz_B_Wl4pc)

### 等待事件

虚拟 DOM 只有在标记为脏时才会重新渲染 `Scope`。每个钩子负责在状态发生变化时标记 `Scope` 为脏。钩子可以通过向虚拟 DOM 的通道发送消息来标记作用域为脏。你可以查看 [implementations](https://github.com/DioxusLabs/dioxus/tree/main/packages/hooks) 了解 Dioxus 默认包含的钩子是如何执行此操作的。调用 `needs_update()` 钩子也会导致它标记其作用域为脏。

通常，作用域被标记为脏有两种方式：

1. 渲染器触发事件：此事件上的事件监听器可能会被调用，如果处理事件导致生成任何突变，则可能会将组件标记为脏。
2. 渲染器调用
   [`wait_for_work`](https://docs.rs/dioxus/latest/dioxus/prelude/struct.VirtualDom.html#method.wait_for_work)：
   这会轮询 Dioxus 内部未来队列。这些未来之一可能会标记组件为脏。

一旦至少有一个 `Scope` 被标记为脏，渲染器就可以调用 [`render_immediate`](https://docs.rs/dioxus/latest/dioxus/prelude/struct.VirtualDom.html#method.render_immediate) 来对脏作用域进行差异比较。

### 对作用域进行差异比较

当用户单击“向上”按钮时，根 `Scope` 将被 `use_signal` 钩子标记为脏。然后，桌面渲染器将调用 `render_immediate`，它将对根 `Scope` 进行差异比较。

为了开始差异比较过程，将运行组件函数。在运行根组件之后，根 `Scope` 将如下所示：

[![](https://mermaid.ink/img/pako:eNrFVlFP2zAQ_iuen0BrpCaIl0i8AEJ72KQJtpcRFBnbJVYTn-U4tBXw33dpG5M2CetoBfdkny_ffb67fPIT5SAkjekkhxnPmHXk-3WiCVpZ3T9YZjJyDeDIDQcjycRCQVwmCTOGXEBhQEvtVvG1CWUldwo0-XX-6vVIF5W1GB9cWVbI1_PNL5v8jW3uPFbpmFOc2HK-GfA2WG1ZeJSFx0EQmJxxmUEupE01liEd394mVAkyjolYaFYgfu1P6N1dF8Yzua-cA51WphtTWzsLc872Zan9CnEGUkktuk6fFm_i5NxFRwn9bUimHrIvCT3-N2EBM70j5XBNOTwI5TrxmvQJkr7ELcHx67Jeggz0v92g8q0RaE-iP1193On6NyxecKUeJeFQaSdtTMLu_Xah5ctT_u94Nty2ZwU0zxWfxqQA5PecPq84kq9nfRw7SK0WDiEFZ4O37d34S_-08lFBVfb92KVb5HIrAp0WpjKYKeGyODLz0dohWIkaZNkiJqfkdLvIH6oRaTSoEmm0n06k0a5K0ZdpL61Io0Yt0nfpxc7UQ0_9cJrhyZ8syX-6brS706Mc489Vjja7fbWj3cxDqIdfJJqOaCFtwZTAV8hT7U0ovjBQRmiMS8HsNKGJfsE4Vjm4WWhOY2crOaKVEczJS8WwgAWNJywv0SuFcmB_rJ41y9fNiBqm_wA0MS9_AUuAiy0?type=png)](https://mermaid.live/edit#pako:eNrFVlFP2zAQ_iuen0BrpCaIl0i8AEJ72KQJtpcRFBnbJVYTn-U4tBXw33dpG5M2CetoBfdkny_ffb67fPIT5SAkjekkhxnPmHXk-3WiCVpZ3T9YZjJyDeDIDQcjycRCQVwmCTOGXEBhQEvtVvG1CWUldwo0-XX-6vVIF5W1GB9cWVbI1_PNL5v8jW3uPFbpmFOc2HK-GfA2WG1ZeJSFx0EQmJxxmUEupE01liEd394mVAkyjolYaFYgfu1P6N1dF8Yzua-cA51WphtTWzsLc872Zan9CnEGUkktuk6fFm_i5NxFRwn9bUimHrIvCT3-N2EBM70j5XBNOTwI5TrxmvQJkr7ELcHx67Jeggz0v92g8q0RaE-iP1193On6NyxecKUeJeFQaSdtTMLu_Xah5ctT_u94Nty2ZwU0zxWfxqQA5PecPq84kq9nfRw7SK0WDiEFZ4O37d34S_-08lFBVfb92KVb5HIrAp0WpjKYKeGyODLz0dohWIkaZNkiJqfkdLvIH6oRaTSoEmm0n06k0a5K0ZdpL61Io0Ytnfpxc7UQ0_9cJrhyZ8syX-6brS706Mc489Vjja7fbWj3cxDqIdfJJqOaCFtwZTAV8hT7U0ovjBQRmiMS8HsNKGJfsE4Vjm4WWhOY2crOaKVEczJS8WwgAWNJywv0SuFcmB_rJ41y9fNiBqm_wA0MS9_AUuAiy0)

接下来，虚拟 DOM 将比较新的 VNode 和之前的 VNode，只更新树中发生变化的部分。由于这种方法，当组件重新渲染时，只有发生变化的树部分才会被渲染器更新到 DOM 中。

差异比较算法会遍历动态属性和节点列表，并将它们与之前的 VNode 进行比较。如果属性或节点发生了变化，一个描述该变化的突变将被添加到突变列表中。

以下是根 `Scope` 的差异比较算法（红线表示生成了突变，绿线表示没有生成突变）：

[![](https://mermaid.ink/img/pako:eNrFlFFPwjAQx7_KpT7Kko2Elya8qCE-aGLAJ5khpe1Yw9Zbug4k4He3OJjbGPig0T5t17tf_nf777aEo5CEkijBNY-ZsfAwDjW4kxfzhWFZDGNECxOOmYTIYAo2lsCyDG4xzVBLbcv8_RHKSG4V6orSIN0Wxrh8b2RYKr_uTyubd1W92GiWKg7aac6bOU3G803HbVk82xfP_Ok0JEqAT-FeLWJvpFYSOBbaSkMhCMnra5MgtfhWFrPWqHlhL2urT6atbU-oa0PNE8WXFFJ0-nazXakRroddGk9IwYEUnCd5w7Pddr5UTT8ZuVJY5F0fM7ebRLYyXNDgUnprJWxM-9lb7xAQLHe-M2xDYQCD9pD_2hez_kVn-P_rjLq6n3qjYv2iO5qz9DyvPdyv1ETp5eTTJ_7BGvQq8v1TVtl5jXUcRRcrqFh-dI4VtFlBN6t_ynLNkh5JpUmZEm5rbvfhkLiN6H4BQt2jYGYZklC_uzxWWJxsNCfUmkL2SJEJZuWdYs4cKaERS3IXlUJZNI_lGv7cxj2SMf2CeMx5_wBcbK19?type=png)](https://mermaid.live/edit#pako:eNrFlFFPwjAQx7_KpT7Kko2Elya8qCE-aGLAJ5khpe1Yw9Zbug4k4He3OJjbGPig0T5t17tf_nf777aEo5CEkijBNY-ZsfAwDjW4kxfzhWFZDGNECxOOmYTIYAo2lsCyDG4xzVBLbcv8_RHKSG4V6orSIN0Wxrh8b2RYKr_uTyubd1W92GiWKg7aac6bOU3G803HbVk82xfP_Ok0JEqAT-FeLWJvpFYSOBbaSkMhCMnra5MgtfhWFrPWqHlhL2urT6atbU-oa0PNE8WXFFJ0-nazXakRroddGk9IwYEUnCd5w7Pddr5UTT8ZuVJY5F0fM7ebRLYyXNDgUnprJWxM-9lb7xAQLHe-M2xDYQCD9pD_2hez_kVn-P_rjLq6n3qjYv2iO5qz9DyvPdyv1ETp5eTTJ_7BGvQq8v1TVtl5jXUcRRcrqFh-dI4VtFlBN6t_ynLNkh5JpUmZEm5rbvfhkLiN6H4BQt2jYGYZklC_uzxWWJxsNCfUmkL2SJEJZuWdYs4cKaERS3IXlUJZNI_lGv7cxj2SMf2CeMx5_wBcbK19)

## 结论

这仅仅是对虚拟 DOM 如何运作的简要概述。本指南中尚未涵盖几个方面，包括：

* 虚拟 DOM 如何处理异步组件。
* 带键的差异比较。
* 使用 [bump allocation](https://github.com/fitzgen/bumpalo) 有效地分配 VNode。

如果你需要更多关于虚拟 DOM 的信息，可以阅读 [core](https://github.com/DioxusLabs/dioxus/tree/main/packages/core) 库的代码，或者在 [Discord](https://discord.gg/XgGxMSkvUM) 联系我们。