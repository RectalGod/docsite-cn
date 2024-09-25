# 移动应用

使用 Dioxus 构建移动应用！

示例：[Mobile Demo](https://github.com/DioxusLabs/dioxus/tree/v0.5/examples/mobile_demo)

## 支持

移动端目前是 Dioxus 支持最少的渲染目标。移动应用使用平台的 WebView 或实验性的 [WGPU](https://github.com/DioxusLabs/blitz) 渲染。WebView 不支持动画、透明度和原生小部件。


移动端支持目前最适合 CRUD 类型的应用，理想情况下适合需要快速开发但不关心动画或原生小部件的内部团队。

## 设置

使用移动端进行设置可能非常具有挑战性。此处的工具（目前）不是很好，可能需要一些调整才能让事情正常运行。

### 设置依赖项

#### Android

首先，安装 rust Android 目标：

```sh
rustup target add aarch64-linux-android armv7-linux-androideabi i686-linux-android x86_64-linux-android
```

要在 Android 上进行开发，您需要 [install Android Studio](https://developer.android.com/studio)。

安装 Android Studio 后，您需要安装 Android SDK 和 NDK：

1. 创建一个空白的 Android 项目
2. 选择 `Tools > SDK manager`
3. 导航到 `SDK tools` 窗口：

![NDK install window](./public/static/android_ndk_install.png)

然后选择：
- SDK
- SDK 命令行工具
- NDK（并行）
- CMAKE

4. 选择 `apply` 并按照提示操作

> 有关调试您遇到的任何错误的更多详细信息，请访问 [in the official android docs](https://developer.android.com/studio/intro/update#sdk-manager)

接下来，设置 Java、Android 和 NDK 主机变量：

Mac：
```sh
export JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home"
export ANDROID_HOME="$HOME/Library/Android/sdk"
export NDK_HOME="$ANDROID_HOME/ndk/25.2.9519653"
```

Windows：
```powershell
[System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Android\Android Studio\jbr", "User")
[System.Environment]::SetEnvironmentVariable("ANDROID_HOME", "$env:LocalAppData\Android\Sdk", "User")
[System.Environment]::SetEnvironmentVariable("NDK_HOME", "$env:LocalAppData\Android\Sdk\ndk\25.2.9519653", "User")
```

> 路径中的 NDK 版本应与您在最后一步安装的版本匹配

#### IOS

要在 IOS 上进行开发，您需要 [install XCode](https://apps.apple.com/us/app/xcode/id497799835)。

> 如果您使用的是 M1，则需要运行 `cargo build --target x86_64-apple-ios` 而不是 `cargo apple build`，如果您想在模拟器中运行。

### 设置您的项目

首先，我们需要创建一个 rust 项目：

```sh
cargo new dioxus-mobile-test --lib
cd dioxus-mobile-test
```

接下来，我们可以使用 `cargo-mobile2` 为移动端创建一个项目：

```shell
cargo install --git https://github.com/tauri-apps/cargo-mobile2
cargo mobile init
```

当您运行 `cargo mobile init` 时，系统会询问您有关项目的一系列问题。其中一个问题是您应该使用什么模板。Dioxus 目前在 Tauri 移动端没有模板，您可以使用 `wry` 模板。

> 您可能还会被要求输入您的 IOS 团队 ID。您可以在 [here](https://developer.apple.com/help/account/manage-your-team/locate-your-team-id/) 找到您的团队 ID，或者通过创建开发者帐户来创建团队 ID [here](https://developer.apple.com/help/account/get-started/about-your-developer-account)

接下来，我们需要修改我们的依赖项以包含 dioxus 并确保安装了正确版本的 wry。更改您 `Cargo.toml` 文件的 `[dependencies]` 部分：

```toml
[dependencies]
anyhow = "1.0"
log = "0.4"
wry = "0.37"
tao = "0.26"
dioxus = { version = "0.6", features = ["mobile"] }
```

最后，我们需要向渲染器添加一个组件。用这段代码替换您 `lib.rs` 文件中的 wry 模板：

```rust
use anyhow::Result;
use dioxus::prelude::*;

#[cfg(target_os = "android")]
fn init_logging() {
    android_logger::init_once(
        android_logger::Config::default()
            .with_max_level(log::LevelFilter::Trace)
    );
}

#[cfg(not(target_os = "android"))]
fn init_logging() {
    env_logger::init();
}

#[cfg(any(target_os = "android", target_os = "ios"))]
fn stop_unwind<F: FnOnce() -> T, T>(f: F) -> T {
    match std::panic::catch_unwind(std::panic::AssertUnwindSafe(f)) {
        Ok(t) => t,
        Err(err) => {
            eprintln!("attempt to unwind out of `rust` with err: {:?}", err);
            std::process::abort()
        }
    }
}

#[no_mangle]
#[inline(never)]
#[cfg(any(target_os = "android", target_os = "ios"))]
pub extern "C" fn start_app() {
    fn _start_app() {
        stop_unwind(|| main().unwrap());
    }

    #[cfg(target_os = "android")]
    {
        tao::android_binding!(
            com_example,
            dioxus_mobile_test,
            WryActivity,
            wry::android_setup, // pass the wry::android_setup function to tao which will invoke when the event loop is created
            _start_app
        );
        wry::android_binding!(com_example, dioxus_mobile_test);
    }
    #[cfg(target_os = "ios")]
    _start_app()
}

pub fn main() -> Result<()> {
    init_logging();

    launch(app);

    Ok(())
}

fn app() -> Element {
    let mut items = use_signal(|| vec![1, 2, 3]);

    log::debug!("Hello from the app");

    rsx! {
        div {
            h1 { "Hello, Mobile"}
            div { margin_left: "auto", margin_right: "auto", width: "200px", padding: "10px", border: "1px solid black",
                button {
                    onclick: move|_| {
                        println!("Clicked!");
                        let mut items_mut = items.write();
                        let new_item = items_mut.len() + 1;
                        items_mut.push(new_item);
                        println!("Requested update");
                    },
                    "Add item"
                }
                for item in items.read().iter() {
                    div { "- {item}" }
                }
            }
        }
    }
}
```

## 运行

从那里，您需要使用您所针对的平台（模拟器或实际硬件）获取该板条箱的构建版本。现在，我们只坚持使用模拟器。

首先，您需要确保 Android Studio 中的构建变体是正确的：
1. 点击顶部菜单栏中的“构建”。
2. 点击下拉菜单中的“选择构建变体...”。
3. 找到“构建变体”面板，使用下拉菜单更改选定的构建变体。

![android studio build dropdown](./public/static/as-build-dropdown.png)
![android studio build variants](./public/static/as-build-variant-menu.png)

### Android

要在 Android 上构建您的项目，您可以运行：

```sh
cargo android build
```

接下来，打开 Android Studio：
```sh
cargo android open
```

这将打开一个用于此应用程序的 Android Studio 项目。

接下来，我们需要在 Android Studio 中创建一个模拟器来运行我们的应用。要创建模拟器，请点击 Android Studio 右上角的手机图标：

![android studio manage devices](./public/static/android-studio-simulator.png)

然后点击 `create a virtual device` 按钮并按照提示操作：

![android studio devices](./public/static/android-studio-devices.png)

最后，通过点击您创建的设备上的播放按钮启动您的设备：

![android studio device](./public/static/android-studio-device.png)

现在，您可以从终端启动应用程序，方法是运行：

```sh
cargo android run
```

![android_demo](./public/static/Android-Dioxus-demo.png)

> Android 文档中提供了更多信息：
> - https://developer.android.com/ndk/guides
> - https://developer.android.com/studio/projects/install-ndk
> - https://source.android.com/docs/setup/build/rust/building-rust-modules/overview

### IOS

要在 IOS 上构建您的项目，您可以运行：
```sh
cargo build --target aarch64-apple-ios-sim
```

接下来，打开 XCode（如果您之前从未打开过 XCode，这可能需要一段时间）：
```sh
cargo apple open
```

这将打开包含此特定项目的 XCode。

从那里，只需点击具有正确目标的“播放”按钮，该应用程序应该就会运行！

![ios_demo](./public/static/IOS-dioxus-demo.png)

请注意，点击播放不会导致新的构建，因此您需要在每次更改之间重新构建应用程序。此处的工具还很年轻，所以请耐心等待。如果您想为简化流程做出贡献，请这样做！我们很乐意提供帮助。