# 选择 Web 渲染器

Dioxus 有三个针对 Web 的不同渲染器：

- [dioxus-web](web.md) 允许您使用客户端上的 [WebAssembly](https://rustwasm.github.io/docs/book/) 将您的应用程序渲染到 HTML。
- [dioxus-liveview](liveview.md) 允许您在服务器上运行您的应用程序，并使用 websocket 将其渲染到客户端的 HTML。
- [dioxus-fullstack](fullstack.md) 允许您在服务器上最初渲染静态 HTML，然后使用 [WebAssembly](https://rustwasm.github.io/docs/book/) 从客户端更新该 HTML。

每种方法都有其优缺点：

### Dioxus Liveview

- Liveview 渲染通过 WebSocket 连接与服务器通信。它本质上将客户端渲染所做的所有工作转移到服务器。

- 这使得**与服务器的通信变得容易，但与客户端/浏览器 API 的通信变得更加困难**。

- 每次交互都需要向服务器发送并接收消息，这会导致**延迟问题**。

- 由于 Liveview 使用 websocket 进行渲染，因此页面将在建立 WebSocket 连接并从 websocket 发送第一个渲染器之前一直为空白。就像客户端渲染一样，这会使页面**对 SEO 不友好**。

- 由于页面是在服务器上渲染的，并且页面是分段发送到客户端的，因此您永远不需要将整个应用程序发送到客户端。对于大型应用程序，初始加载时间可能比客户端渲染更快，因为 Liveview 只需要发送一个固定的、较小的 websocket 脚本，而不管应用程序的大小。

> Liveview 非常适合那些已经需要频繁与服务器通信（如实时协作应用程序）但不需要与太多客户端/浏览器 API 通信的应用程序。

[![](https://mermaid.ink/img/pako:eNplULFOw0AM_RXLc7Mw3sBQVUIMRYgKdcli5ZzkRHIuPl8QqvrvXJICRXiy3nt-9-6dsRHP6DAZGe8CdUpjNd3VEcpsVT4SK1TVPRxYJ1YHL_yeOdkqWMGF3w4U32Y6nSQmXvknMQYNXW8g7bfk2JPBg0g3MCTmdH1rJhenx2is1FiYri43wJ8or3O2H1Liv0w3hw724kMb2MMzdcUYNziyjhR8-f15Pq3Reh65RldWzy3lwWqs46VIKZscPmODzjTzBvPJ__aFrqUhFZR9MNH92uhS7OULYSF1lw?type=png)](https://mermaid.live/edit#pako:eNplULFOw0AM_RXLc7Mw3sBQVUIMRYgKdcli5ZzkRHIuPl8QqvrvXJICRXiy3nt-9-6dsRHP6DAZGe8CdUpjNd3VEcpsVT4SK1TVPRxYJ1YHL_yeOdkqWMGF3w4U32Y6nSQmXvknMQYNXW8g7bfk2JPBg0g3MCTmdH1rJhenx2is1FiYri43wJ8or3O2H1Liv0w3hw724kMb2MMzdcUYNziyjhR8-f15Pq3Reh65RldWzy3lwWqs46VIKZscPmODzjTzBvPJ__aFrqUhFZR9MNH92uhS7OULYSF1lw)

### Dioxus Web

- 使用客户端渲染，您将您的应用程序发送到客户端，然后客户端动态生成页面的所有 HTML。

- 这意味着页面将在 JavaScript 包加载并应用程序初始化之前一直为空白。这会导致**首次渲染时间变慢以及 SEO 性能低下**。

> SEO 代表搜索引擎优化。它指的是使您的网站更有可能出现在搜索引擎结果中的实践。像 Google 和 Bing 这样的搜索引擎使用网络爬虫来索引网站内容。大多数这些爬虫无法运行 JavaScript，因此如果您的页面是在客户端渲染的，它们将无法索引您的页面内容。

- 客户端渲染的应用程序需要使用**弱类型请求与服务器通信**。

> 客户端渲染是大多数应用程序的良好起点。它得到了很好的支持，并使与客户端/浏览器 API 的通信变得容易。

[![](https://mermaid.ink/img/pako:eNpVkDFPwzAQhf-KdXOzMHpgqJAQAwytEIsXK35JLBJfez4Xoar_HSemQtzke9_z2e-u1HMAWcrqFU_Rj-KX7vLgkqm1F_7KENN1j-YIuUCsOeBckLUZmrjx_ezT54rziVNG42-sMBLHSQ0Pd8vH5NU8M48zTAby71sr3CYdkAIEoen37h-y5n3910tSiO81cqIdLZDFx1DDXNerjnTCAke2HgMGX2Z15NKtWn1RPn6nnqxKwY7KKfzFJzv4OVcVISrLa1vQtqfbDzd0ZKY?type=png)](https://mermaid.live/edit#pako:eNpVkDFPwzAQhf-KdXOzMHpgqJAQAwytEIsXK35JLBJfez4Xoar_HSemQtzke9_z2e-u1HMAWcrqFU_Rj-KX7vLgkqm1F_7KENN1j-YIuUCsOeBckLUZmrjx_ezT54rziVNG42-sMBLHSQ0Pd8vH5NU8M48zTAby71sr3CYdkAIEoen37h-y5n3910tSiO81cqIdLZDFx1DDXNerjnTCAke2HgMGX2Z15NKtWn1RPn6nnqxKwY7KKfzFJzv4OVcVISrLa1vQtqfbDzd0ZKY)

### Dioxus 全栈

全栈渲染分为两个部分：

1. 页面在服务器上渲染。这可能包括获取您渲染页面所需的任何数据。
2. 页面在客户端上进行水合。 （水合是指获取来自服务器的 HTML 页面并在客户端添加 Dioxus 需要的所有事件监听器）。从这一点开始，页面的任何更新都将在客户端上发生。

由于页面最初是在服务器上渲染的，因此页面在发送到客户端时将完全渲染。这会导致更快的首次渲染时间，并使页面对 SEO 更友好。

- **快速的初始渲染**
- **与 SEO 兼容**
- **类型安全的、轻松的服务器通信**
- **访问客户端/浏览器 API**
- **快速的交互性**

最后，我们可以使用 [server functions](../reference/fullstack/server_functions.md) 以类型安全的方式与服务器通信。

此方法使用 dioxus-web 和 dioxus-ssr 两个 crate。为了集成这两个包，Dioxus 提供了 `dioxus-fullstack` crate。

全栈应用程序可能更复杂，因为您的代码在两个不同的位置运行。Dioxus 尝试使用服务器函数和其他助手来减轻这种情况。

[![](https://mermaid.ink/img/pako:eNpdkL1uwzAMhF9F4BwvHTV0KAIUHdohQdFFi2CdbQG2mFCUiyDIu9e2-hOUE3H34UDelVoOIEtZvWIffS9-auYHl8wyT8KfGWKa5tEcITPEmgPOBVkrUMXNPyAFCMJK5BOnjIq8scJI7Ac13N1RH4NX88zcjzAZyJX-8bfIl6QQ32qcv7PuhP-ANe_rpb8KJ9rRBJl8DMt71zXAkQ6Y4Mgua0Dny6iOXLotqC_Kx0tqyaoU7Kicwl8hZDs_5kVFiMryWivbmrt9AacxbGg?type=png)](https://mermaid.live/edit#pako:eNpdkL1uwzAMhF9F4BwvHTV0KAIUHdohQdFFi2CdbQG2mFCUiyDIu9e2-hOUE3H34UDelVoOIEtZvWIffS9-auYHl8wyT8KfGWKa5tEcITPEmgPOBVkrUMXNPyAFCMJK5BOnjIq8scJI7Ac13N1RH4NX88zcjzAZyJX-8bfIl6QQ32qcv7PuhP-ANe_rpb8KJ9rRBJl8DMt71zXAkQ6Y4Mgua0Dny6iOXLotqC_Kx0tqyaoU7Kicwl8hZDs_5kVFiMryWivbmrt9AacxbGg)