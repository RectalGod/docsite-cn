# 身份验证

您可以使用 [extractors](./extractors) 将身份验证与您的全栈应用程序集成。

您可以创建一个自定义提取器，从请求中提取身份验证会话。从该身份验证会话中，您可以在返回私有数据之前检查用户是否具有所需的权限。

一个包含完整实现的 [full auth example](https://github.com/DioxusLabs/dioxus/blob/v0.5/packages/fullstack/examples/axum-auth/src/main.rs) 在全栈示例中可用。