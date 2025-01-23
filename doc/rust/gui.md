# Dioxus

https://github.com/DioxusLabs/dioxus/tree/main



Build for web, desktop, and mobile, and more with a single codebase. Zero-config setup, integrated hot-reloading, and signals-based state management. Add backend functionality with Server Functions and bundle with our CLI.

```rust
fn app() -> Element {
    let mut count = use_signal(|| 0);

    rsx! {
        h1 { "High-Five counter: {count}" }
        button { onclick: move |_| count += 1, "Up high!" }
        button { onclick: move |_| count -= 1, "Down low!" }
    }
}
```

## ⭐️ Unique features:

- Cross-platform apps in three lines of code (web, desktop, mobile, server, and more)
- [Ergonomic state management](https://dioxuslabs.com/blog/release-050) combines the best of React, Solid, and Svelte
- Type-safe Routing and server functions to leverage Rust's powerful compile-time guarantees
- Integrated bundler for deploying to the web, macOS, Linux, and Windows
- And more! [Take a tour of Dioxus](https://dioxuslabs.com/learn/0.6/).

## Instant hot-reloading

With one command, `dx serve` and your app is running. Edit your markup and styles and see the results in real time.

<div align="center">
  <img src="https://raw.githubusercontent.com/DioxusLabs/screenshots/refs/heads/main/blitz/hotreload-video.webp">
  <!-- <video src="https://private-user-images.githubusercontent.com/10237910/386919031-6da371d5-3340-46da-84ff-628216851ba6.mov" width="500"></video> -->
  <!-- <video src="https://private-user-images.githubusercontent.com/10237910/386919031-6da371d5-3340-46da-84ff-628216851ba6.mov" width="500"></video> -->
</div>


## First-class Android and iOS support

Dioxus is the fastest way to build native mobile apps with Rust. Simply run `dx serve --platform android` and your app is running in an emulator or on device in seconds. Call directly into JNI and Native APIs.

<div align="center">
  <img src="./notes/android_and_ios2.avif" width="500">
</div>

## Bundle for web, desktop, and mobile

Simply run `dx bundle` and your app will be built and bundled with maximization optimizations. On the web, take advantage of [`.avif` generation, `.wasm` compression, minification](https://dioxuslabs.com/learn/0.6/guides/assets), and more. Build WebApps weighing [less than 50kb](https://github.com/ealmloff/tiny-dioxus/) and desktop/mobile apps less than 5mb.

<div align="center">
  <img src="./notes/bundle.gif">
</div>


## Fantastic documentation

We've put a ton of effort into building clean, readable, and comprehensive documentation. All html elements and listeners are documented with MDN docs, and our Docs runs continuous integration with Dioxus itself to ensure that the docs are always up to date. Check out the [Dioxus website](https://dioxuslabs.com/learn/0.6/) for guides, references, recipes, and more. Fun fact: we use the Dioxus website as a testbed for new Dioxus features - [check it out!](https://github.com/dioxusLabs/docsite)

<div align="center">
  <img src="./notes/docs.avif">
</div>

## Community

Dioxus is a community-driven project, with a very active [Discord](https://discord.gg/XgGxMSkvUM) and [GitHub](https://github.com/DioxusLabs/dioxus/issues) community. We're always looking for help, and we're happy to answer questions and help you get started. [Our SDK](https://github.com/DioxusLabs/dioxus-std) is community-run and we even have a [GitHub organization](https://github.com/dioxus-community/) for the best Dioxus crates that receive free upgrades and support.

<div align="center">
  <img src="./notes/dioxus-community.avif">
</div>

## Full-time core team

Dioxus has grown from a side project to a small team of fulltime engineers. Thanks to the generous support of FutureWei, Satellite.im, the GitHub Accelerator program, we're able to work on Dioxus full-time. Our long term goal is for Dioxus to become self-sustaining by providing paid high-quality enterprise tools. If your company is interested in adopting Dioxus and would like to work with us, please reach out!

## Supported Platforms

<div align="center">
  <table style="width:100%">
    <tr>
      <td>
      <b>Web</b>
      </td>
      <td>
        <ul>
          <li>Render directly to the DOM using WebAssembly</li>
          <li>Pre-render with SSR and rehydrate on the client</li>
          <li>Simple "hello world" at about 50kb, comparable to React</li>
          <li>Built-in dev server and hot reloading for quick iteration</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
      <b>Desktop</b>
      </td>
      <td>
        <ul>
          <li>Render using Webview or - experimentally - with WGPU or <a href="https://freyaui.dev">Freya</a> (Skia) </li>
          <li>Zero-config setup. Simply `cargo run` or `dx serve` to build your app </li>
          <li>Full support for native system access without IPC </li>
          <li>Supports macOS, Linux, and Windows. Portable <3mb binaries </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
      <b>Mobile</b>
      </td>
      <td>
        <ul>
          <li>Render using Webview or - experimentally - with WGPU or Skia </li>
          <li>Build .ipa and .apk files for iOS and Android </li>
          <li>Call directly into Java and Objective-C with minimal overhead</li>
          <li>From "hello world" to running on device in seconds</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
      <b>Server-side Rendering</b>
      </td>
      <td>
        <ul>
          <li>Suspense, hydration, and server-side rendering</li>
          <li>Quickly drop in backend functionality with server functions</li>
          <li>Extractors, middleware, and routing integrations</li>
          <li>Static-site generation and incremental regeneration</li>
        </ul>
      </td>
    </tr>
  </table>
</div>

## Running the examples

> The examples in the main branch of this repository target the git version of dioxus and the CLI. If you are looking for examples that work with the latest stable release of dioxus, check out the [0.6 branch](https://github.com/DioxusLabs/dioxus/tree/v0.6/examples).

The examples in the top level of this repository can be run with:

```sh
cargo run --example <example>
```

However, we encourage you to download the dioxus-cli. If you are running the git version of dioxus, you can install the matching version of the CLI with:

```sh
cargo install --git https://github.com/DioxusLabs/dioxus dioxus-cli --locked
```

With the CLI, you can also run examples with the web platform. You just need to disable the default desktop feature and enable the web feature with this command:

```sh
dx serve --example <example> --platform web -- --no-default-features
```

## Dioxus vs other frameworks

We love all frameworks and enjoy watching innovation in the Rust ecosystem. In fact, many of our projects are shared with other frameworks. For example, our flex-box library [Taffy](https://github.com/DioxusLabs/taffy) is used by [Bevy](https://bevyengine.org/), [Zed](https://zed.dev/), [Lapce](https://lapce.dev/), [Iced](https://github.com/iced-rs/iced), and many more.

Dioxus places an emphasis on a few key points that make it different from other frameworks:

- **React-like**: we rely on concepts like components, props, and hooks to build UIs, with our state management being closer to Svelte than to SolidJS.
- **HTML and CSS**: we lean completely into HTML and CSS, quirks and all.
- **Renderer-agnostic**: you can swap out the renderer for any platform you want thanks to [our fast VirtualDOM](https://dioxuslabs.com/blog/templates-diffing).
- **Collaborative**: whenever possible, we spin out crates like [Taffy](https://github.com/DioxusLabs/taffy), [manganis](https://github.com/DioxusLabs/manganis), [include_mdbook](https://github.com/DioxusLabs/include_mdbook), and [blitz](http://github.com/dioxusLabs/blitz) so the ecosystem can grow together.

### Dioxus vs Tauri

Tauri is a framework for building desktop mobile apps where your frontend is written in a web-based framework like React, Vue, Svelte, etc. Whenever you need to do native work, you can write Rust functions and call them from your frontend.

- **Natively Rust**: Tauri's architecture limits your UI to either JavaScript or WebAssembly. With Dioxus, your Rust code is running natively on the user's machine, letting you do things like spawning threads, accessing the filesystem, without any IPC bridge. This drastically simplifies your app's architecture and makes it easier to build. You can build a Tauri app with Dioxus-Web as a frontend if you'd like.

- **Different scopes**: Tauri needs to support JavaScript and its complex build tooling, limiting the scope of what you can do with it. Since Dioxus is exclusively focused on Rust, we're able to provide extra utilities like Server Functions, advanced bundling, and a native renderer.

- **Shared DNA**: While Tauri and Dioxus are separate projects, they do share libraries like Tao and Wry: windowing and webview libraries maintained by the Tauri team.

### Dioxus vs Leptos

Leptos is a library for building fullstack web-apps, similar to SolidJS and SolidStart. The two libraries share similar goals on the web, but have several key differences:

- **Reactivity model**: Leptos uses signals to drive both reactivity and rendering, while Dioxus uses signals just for reactivity. For managing re-renders, Dioxus uses a highly optimized VirtualDOM to support desktop and mobile architectures. Both Dioxus and Leptos are extremely fast.

- **Different scopes**: Dioxus provides renderers for web, desktop, mobile, LiveView, and more. We also maintain community libraries and a cross-platform SDK. Leptos has a tighter focus on the fullstack web with features that Dioxus doesn't have like islands, `<Form />` components, and other web-specific utilities.

- **Different DSLs**: Dioxus uses its own custom Rust-like DSL for building UIs while Leptos uses an HTML-like syntax. We chose this to retain compatibility with IDE features like code-folding and syntax highlighting. Generally, Dioxus leans into more "magic" with its DSL including automatic formatting of strings and hot-reloading of simple Rust expressions.

```rust
// dioxus
rsx! {
  div {
    class: "my-class",
    enabled: true,
    "Hello, {name}"
  }
}

// leptos
view! {
  <div class="my-class" enabled={true}>
    "Hello "
    {name}
  </div>
}
```

### Dioxus vs Yew

Yew is a framework for building single-page web apps and initially served as an inspiration for Dioxus. Unfortunately, the architecture of Yew didn't support the various features we wanted, and thus Dioxus was born.

- **Single-page apps**: Yew is designed exclusively for single-page web apps and is intrinsically tied to the web platform. Dioxus is fullstack and cross-platform, making it suitable for building web, desktop, mobile, and server apps.

- **Developer Tooling**: Dioxus provides a number of utilities like autoformatting, hot-reloading, and a bundler.

- **Ongoing support**: Dioxus is very actively maintained with new features and bug fixes being added on a daily basis.

### Dioxus vs egui

egui is a cross-platform GUI library for Rust powering tools like [Rerun.io](https://www.rerun.io).

- **Immediate vs Retained**: egui is designed to be re-rendered on every frame. This is suitable for games and other interactive applications, but it does not retain style and layout state between frames. Dioxus is a retained UI framework, meaning that the UI is built once and then modified between frames. This enables Dioxus to use native web technologies like HTML and CSS with better battery life and performance.

- **Customizable**: egui brings its own styling and layout solution while Dioxus expects you to use the built-in HTML and CSS. This enables dioxus apps to use any CSS library like Tailwind or Material UI.

- **State management**: egui's state management is based on a single global state object. Dioxus encourages encapsulation of state by using components and props, making components more reusable.

### Dioxus vs Iced

Iced is a cross-platform GUI library inspired by Elm. Iced renders natively with WGPU and supports the web using DOM nodes.

- **Elm state management**: Iced uses Elm's state management model, which is based on message passing and reducers. This is simply a different state management model than Dioxus and can be rather verbose at times.

- **Native Feel**: Since Dioxus uses a webview as its renderer, it automatically gets native text input, paste handling, and other native features like accessibility. Iced's renderer currently doesn't implement these features, making it feel less native.

- **WGPU**: Dioxus' WGPU renderer is currently quite immature and not yet ready for production use. Iced's WGPU renderer is much more mature and is being used in production. This enables certain types of apps that need GPU access to be built with Iced that can't currently be built with Dioxus.

### Dioxus vs Electron

Dioxus and Electron are two entirely different projects with similar goals. Electron makes it possible for developers to build cross-platform desktop apps using web technologies like HTML, CSS, and JavaScript.

- **Lightweight**: Dioxus uses the system's native WebView - or optionally, a WGPU renderer - to render the UI. This makes a typical Dioxus app about 15mb on macOS in comparison to Electron's 100mb. Electron also ships an embedded chromium instance which cannot share system resources with the host OS in the same way as Dioxus.

- **Maturity**: Electron is a mature project with a large community and a lot of tooling. Dioxus is still quite young in comparison to Electron. Expect to run into features like deep-linking that require extra work to implement.






# FREYA


https://github.com/marc2332/freya/tree/main





# Freya 🦀


**Freya** is a cross-platform GUI library for Rust powered by 🧬 [Dioxus](https://dioxuslabs.com) and 🎨 [Skia](https://skia.org/).

**It does not use any web tech**, check the [Differences with Dioxus](https://book.freyaui.dev/differences_with_dioxus.html).

⚠️ It's currently work in progress, but you can already play with it! You can join the [Discord](https://discord.gg/sYejxCdewG) server if you have any question or issue.

<br/>

<table>
<tr>
<td style="border:hidden;">

```rust
fn app() -> Element {
    let mut count = use_signal(|| 0);

    rsx!(
        rect {
            height: "50%",
            width: "100%",
            main_align: "center",
            cross_align: "center",
            background: "rgb(0, 119, 182)",
            color: "white",
            shadow: "0 4 20 5 rgb(0, 0, 0, 80)",
            label {
                font_size: "75",
                font_weight: "bold",
                "{count}"
            }
        }
        rect {
            height: "50%",
            width: "100%",
            main_align: "center",
            cross_align: "center",
            direction: "horizontal",
            Button {
                onclick: move |_| count += 1,
                label { "Increase" }
            }
            Button {
                onclick: move |_| count -= 1,
                label { "Decrease" }
            }
        }
    )
}
```
</td>
<td style="border:hidden;">

![Freya Demo](https://github.com/marc2332/freya/assets/38158676/f81a95a2-7add-4dbe-9820-3d3b6b42f6e5)

</td>
</table>

### Want to try it? 🤔

👋 Make sure to check the [Setup guide](https://book.freyaui.dev/setup.html) first.

> ⚠️ If you happen to be on Windows using `windows-gnu` and get compile errors, maybe go check this [issue](https://github.com/marc2332/freya/issues/794).

Clone this repo and run:

```shell
cargo run --example counter
```

You can also try [`freya-template`](https://github.com/marc2332/freya-template)

### Usage 📜
Add Freya and Dioxus as dependencies:

```toml
freya = "0.2"
dioxus = { version = "0.5", features = ["macro", "hooks"], default-features = false }
```
### Contributing 🧙‍♂️

If you are interested in contributing please make sure to have read the [Contributing](CONTRIBUTING.md) guide first!

### Features ✨
- ⛏️ Built-in **components** (button, scroll views, switch and more)
- 🚇 Built-in **hooks** (animations, text editing and more)
- 🔍 Built-in **developer tools** (tree inspection, fps overlay)
- 🧰 Built-in **headless runner** to test UI
- 🎨 **Theming** support
- 🛩️ **Cross-platform** (Windows, Linux, MacOS)
- 🖼️ SKSL **Shaders** support
- 📒 Multi-line **text editing**
- 🦾 **Accessibility** support
- 🧩 Compatible with dioxus-sdk and other Dioxus renderer-agnostic libraries

### Goals 😁
- Performant and low memory usage
- Good developer experience
- Cross-platform support
- Decent Accessibility support
- Useful testing APIs
- Useful and extensible built-in components and hooks

### Support 🤗

If you are interested in supporting the development of this project feel free to donate to my [Github Sponsor](https://github.com/sponsors/marc2332/) page.

Thanks to my sponsors for supporting this project! 😄

<!-- sponsors --><!-- sponsors -->

### Special thanks 💪

- [Jonathan Kelley](https://github.com/jkelleyrtp) and [Evan Almloff](https://github.com/ealmloff) for making [Dioxus](https://dioxuslabs.com/) and all their help, specially when I was still creating Freya.
- [Armin](https://github.com/pragmatrix) for making [rust-skia](https://github.com/rust-skia/rust-skia/) and all his help and making the favor of hosting prebuilt binaries of skia for the combo of features use by Freya.
- [geom3trik](https://github.com/geom3trik) for helping me figure out how to add incremental rendering.
- [Tropical](https://github.com/Tropix126) for this contributions to improving accessibility and rendering.
- And to the rest of contributors and anybody who gave me any kind of feedback!

### 🤠 Projects

[Valin](https://github.com/marc2332/valin) ⚒️ is a Work-In-Progress cross-platform code editor, made with Freya 🦀 and Rust, by me.

![Valin](https://github.com/marc2332/valin/raw/main/demo.png)

