# UIs

7 GUI Tasks

https://eugenkiss.github.io/7guis/tasks


## See [Dioxus / Freya](gui.md)

## floem

https://github.com/lapce/floem?tab=readme-ov-file

A native Rust UI library with fine-grained reactivity


### Features

Inspired by Xilem, Leptos and rui, Floem aims to be a high performance declarative UI library with a highly ergonomic API.

- Cross-platform: Floem supports Windows, macOS and Linux with rendering using wgpu. In case a GPU is unavailable, a CPU renderer powered by tiny-skia will be used.
- Fine-grained reactivity: The entire library is built around reactive primitives inspired by leptos_reactive. The reactive "signals" allow you to keep your UI up-to-date with minimal effort, all while maintaining very high performance.
- Performance: The view tree is constructed only once, safeguarding you from accidentally creating a bottleneck in a view generation function that slows down your entire application. Floem also provides tools to help you write efficient UI code, such as a virtual list.
- Flexbox layout: Using Taffy, the library provides the Flexbox and Grid layout systems, which can be applied to any View node.
- Customizable widgets: Widgets are highly customizable. You can customize both the appearance and behavior of widgets using the styling API, which supports theming with classes. You can also install third-party themes.
- Transitions and Animations: Floem supports both transitions and animations. Transitions, like css transitions, can animate any property that can be interpolated and can be applied alongside other styles, including in classes. Floem also supports full keyframe animations that build on the ergonomics of the style system. In both transitions and animations, Floem supports easing with spring functions.
- Element inspector: Inspired by your browser's developer tools, Floem provides a diagnostic tool to debug your layout.


https://github.com/lapce/floem/blob/main/examples/editor/src/main.rs




## EWW


https://github.com/elkowar/eww?tab=readme-ov-file


Elkowars Wacky Widgets is a standalone widget system made in Rust that allows you to implement your own, custom widgets in any window manager.

Documentation and instructions on how to install can be found here.

Dharmx also wrote a nice, beginner friendly introductory guide for eww here.





# swww

https://github.com/LGFae/swww?tab=readme-ov-file

A Solution to your Wayland Wallpaper Woes
Efficient animated wallpaper daemon for wayland, controlled at runtime






