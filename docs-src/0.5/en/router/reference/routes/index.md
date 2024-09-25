# 定义路由

在创建 `Routable`] 枚举时，我们可以使用 `route("path")` 属性为我们的应用程序定义路由。

## 路由段

每个路由都由段组成。大多数段在路径中由 `/` 字符分隔。

有四种基本的段类型：

1. [Static segments](#static-segments) 是必须出现在路径中的固定字符串。
2. [Dynamic segments](#dynamic-segments) 是可以从一个段中解析的类型。
3. [Catch-all segments](#catch-all-segments) 是可以从多个段中解析的类型。
4. [Query segments](#query-segments) 是可以从查询字符串中解析的类型。

路由匹配：

- 首先，从最具体到最不具体（静态、然后动态、然后通配符）（查询始终匹配）
- 然后，如果多个路由匹配同一条路径，则按照它们在枚举中定义的顺序进行匹配。

## 静态段

固定路由匹配特定的路径。例如，路由 `#[route("/about")]` 将匹配路径 `/about`.

```rust
{{#include src/doc_examples/static_segments.rs:route}}
```

```rust
use axum::{routing::get, Router};

#[derive(Debug, Clone, Copy)]
enum Routes {
    Static,
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(handler))
        .route("/users", get(handler));

    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}

async fn handler() {
    // ...
}
```

## 动态段

动态段的形式为 `:name`，其中 `name` 是路由变体中字段的名称。如果段成功解析，则路由匹配，否则匹配继续。

段可以是任何实现 `FromStr` 的类型。

```rust
{{#include src/doc_examples/dynamic_segments.rs:route}}
```

```rust
use axum::{routing::get, Router};
use std::str::FromStr;

#[derive(Debug, Clone, Copy)]
enum Routes {
    Static,
    Dynamic(u32),
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(handler))
        .route("/users/:id", get(handler));

    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}

async fn handler(
    axum::extract::Path(id): axum::extract::Path<u32>,
) {
    // ...
}
```

## 通配符段

通配符段的形式为 `:..name`，其中 `name` 是路由变体中字段的名称。如果段成功解析，则路由匹配，否则匹配继续。

段可以是任何实现 `FromSegments` 的类型。（Vec<String> 默认实现此接口）

通配符段必须是路径中的 _最后一个路由段_（查询段不算在内），并且不能包含在嵌套中。

```rust
{{#include src/doc_examples/catch_all_segments.rs:route}}
```

```rust
use axum::{routing::get, Router};

#[derive(Debug, Clone, Copy)]
enum Routes {
    Static,
    CatchAll(Vec<String>),
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(handler))
        .route("/users/*path", get(handler));

    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}

async fn handler(
    axum::extract::Path(path): axum::extract::Path<Vec<String>>,
) {
    // ...
}
```

## 查询段

查询段的形式为 `?:name&:othername`，其中 `name` 和 `othername` 是路由变体中字段的名称。

与 [Dynamic Segments](#dynamic-segments) 和 [Catch All Segments](#catch-all-segments) 不同，解析查询段必须不会失败。

段可以是任何实现 `FromQueryArgument` 的类型。

查询段必须在 _所有路由段之后_，并且不能包含在嵌套中。

```rust
{{#include src/doc_examples/query_segments.rs:route}}
```

```rust
use axum::{routing::get, Router};
use std::str::FromStr;

#[derive(Debug, Clone, Copy)]
enum Routes {
    Static,
    Query(u32, String),
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(handler))
        .route("/users", get(handler));

    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}

async fn handler(
    axum::extract::Query(query): axum::extract::Query<(u32, String)>,
) {
    // ...
}
```