# 静态生成

## 获取站点地图

`[`Routable`]` 特性包含一个关联的 `[`SITE_MAP`]` 常量，其中包含枚举中所有路由的映射。

默认情况下，站点地图是（静态或动态）`RouteTypes` 的树，但可以使用 `[`.flatten()`` 方法将其展平为单个路由列表。

## 生成站点地图

要静态渲染页面，我们需要展平路由树并为每个路由生成一个文件，该文件只包含静态片段：

```rust
{{#include src/doc_examples/static_generation.rs}}
```

## 示例

- [examples/static-hydrated](https://github.com/DioxusLabs/dioxus/tree/v0.5/packages/fullstack/examples/static-hydrated)

`[`Routable`]:` https://docs.rs/dioxus-router/latest/dioxus_router/components/fn.Routable.html