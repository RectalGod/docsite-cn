# 嵌套路由

在开发大型应用时，我们经常希望将路由彼此嵌套。例如，我们可能希望使用这种模式来组织设置菜单：

```plain
└ Settings
  ├ General Settings (displayed when opening the settings)
  ├ Change Password
  └ Privacy Settings
```

我们可能希望将此结构映射到这些路径和组件：

```plain
/settings		  -> Settings { GeneralSettings }
/settings/password -> Settings { PWSettings }
/settings/privacy  -> Settings { PrivacySettings }
```

嵌套路由允许我们执行此操作，而无需在每个路由中重复 /settings。

## 嵌套

要嵌套路由，我们使用 `#[nest("path")]` 和 `#[end_nest]` 属性。

nest 中的路径不能：

1. 包含 [Catch All Segment](./#catch-all-segments)
2. 包含 [Query Segment](./#query-segments)

如果你在 nest 中定义了一个动态段，它将对所有子路由和布局可用。

要完成嵌套，我们使用 `#[end_nest]` 属性或枚举的结束。

```rust
{{#include src/doc_examples/nest.rs:route}}
```