# 日志记录

Dioxus 支持多种平台，每个平台都有自己的日志记录需求。我们将讨论为您的项目提供的不同选项。

#### Tracing 框架
[Tracing](https://crates.io/crates/tracing) 框架是 Dioxus 库使用的日志记录接口。不需要使用 Tracing 框架，但您将不会收到来自 Dioxus 库的日志。

Tracing 框架提供各种简单的 `println` 样式的宏，具有不同的严重级别。
可用的宏如下所示，最底部的严重级别最高：
```rs
fn main() {
    tracing::trace!("trace");
    tracing::debug!("debug");
    tracing::info!("info");
    tracing::warn!("warn");
    tracing::error!("error");
}
```
除了配置和初始化之外，此页面上提供的所有记录器都使用这些宏进行交互。您通常还会使用 Tracing 框架的 `Level` 枚举。该枚举通常表示应用程序要发出的最大日志严重级别，并且可以从多种来源加载，例如配置文件、环境变量等等。

有关更多信息，请访问 Tracing 框架的 [docs](https://docs.rs/tracing/latest/tracing/) 页面。

## Dioxus 记录器
[Dioxus Logger](https://crates.io/crates/dioxus-logger) 是一个日志记录实用程序，它将为平台启动相应的记录器。目前除了移动平台之外，所有平台都支持。

要使用 Dioxus 记录器，请调用 `init()` 函数：
```rs
use tracing::Level;

fn main() {
    // Init logger
    dioxus_logger::init(Level::INFO).expect("failed to init logger");
    // Dioxus launch code
}
```
`dioxus_logger::init()` 函数使用默认配置和提供的 `Level` 初始化 Dioxus 记录器，并使用相应的 Tracing 记录器。

#### 平台差异
在 Web 平台上，Dioxus 记录器将使用 [tracing-wasm](https://crates.io/crates/tracing-wasm)。在桌面和基于服务器的目标平台上，Dioxus 记录器将使用 [tracing-subscriber](https://crates.io/crates/tracing-subscriber) 的 `FmtSubscriber`。

#### 最后说明
如果符合您的需求，Dioxus 记录器是与 Dioxus 一起使用的首选记录器。未来会有更多功能，Dioxus 记录器计划成为 Dioxus 不可或缺的一部分。如果有任何功能建议或 Dioxus 记录器问题，请随时在 [Dioxus Discord Server](https://discord.gg/XgGxMSkvUM) 上联系我们！

有关更多信息，请访问 Dioxus 记录器的 [docs](https://docs.rs/dioxus-logger/latest/dioxus_logger/) 页面。

## 桌面和服务器
对于 Dioxus 的桌面和服务器目标平台，您通常可以使用您选择的记录器。

一些流行的选项包括：
- [tracing-subscriber](https://crates.io/crates/tracing-subscriber) 的 `FmtSubscriber` 用于控制台输出。
- [tracing-appender](https://crates.io/crates/tracing-appender) 用于将日志记录到文件。
- [tracing-bunyan-formatter](https://crates.io/crates/tracing-bunyan-formatter) 用于 Bunyan 格式。

为了使本指南简短，我们将不介绍这些框架的使用方法。

有关流行的基于 Tracing 的日志记录框架的完整列表，请访问 Tracing 框架文档中的 [this](https://docs.rs/tracing/latest/tracing/#related-crates) 列表。

## Web
[tracing-wasm](https://crates.io/crates/tracing-wasm) 是一个日志记录接口，可用于 Dioxus 的 Web 平台。

使用 WASM 记录器最简单的方法是使用 `set_as_global_default` 函数：
```rs
fn main() {
    // Init logger
    tracing_wasm::set_as_global_default();
    // Dioxus code
}
```
这将使用 `Level` 的 `Trace` 启动 Tracing。

使用自定义 `level` 会稍微复杂一些。我们需要使用 `WasmLayerConfigBuilder` 并使用 `set_as_global_default_with_config()` 启动记录器：
```rs
use tracing::Level;

fn main() {
    // Init logger
    let tracing_config = tracing_wasm::WASMLayerConfigBuilder::new().set_max_level(Level::INFO).build();
    tracing_wasm::set_as_global_default_with_config(tracing_config);
    // Dioxus code
}
```

# 移动
不幸的是，没有与移动目标平台兼容的 Tracing 框架。作为替代方案，您可以使用 [log](https://crates.io/crates/log) 框架。

## Android
[Android Logger](https://crates.io/crates/android_logger) 是一个日志记录接口，可用于针对 Android 平台。Android 记录器在 Android 系统调用事件 `native_activity_create` 时运行：
```rs
use log::LevelFilter;
use android_logger::Config;

fn native_activity_create() {
    android_logger::init_once(
        Config::default()
            .with_max_level(LevelFilter::Info)
            .with_tag("myapp");
    );
}
```
`with_tag()` 是您的应用程序日志将显示的内容。

#### 查看日志
Android 日志发送到 logcat。要通过 Android 调试器使用 logcat，请运行：
```cmd
adb -d logcat
```
您的 Android 设备需要启用开发者选项/USB 调试。

有关更多信息，请访问 android_logger 的 [docs](https://docs.rs/android_logger/latest/android_logger/) 页面。

## iOS
iOS 目前的选项是 [oslog](https://crates.io/crates/oslog) 框架。

```rs
fn main() {
    // Init logger
    OsLogger::new("com.example.test")
        .level_filter(LevelFilter::Debug)
        .init()
        .expect("failed to init logger");
    // Dioxus code
}
```

#### 查看日志
您可以在 Xcode 中查看发出的日志。

有关更多信息，请访问 [oslog](https://crates.io/crates/oslog)。