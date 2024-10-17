# Pake

https://github.com/tw93/Pake

Turn any webpage into a desktop app with Rust with ease.

Pake supports Mac, Windows, and Linux. Check out README for Popular Packages, Command-Line Packaging, and Customized Development information. Feel free to share your suggestions in Discussions.


Features
- 🎐 Nearly 20 times smaller than an Electron package (around 5M!)
- 🚀 With Rust Tauri, Pake is much more lightweight and faster than JS-based frameworks.
- 📦 Battery-included package — shortcut pass-through, immersive windows, and minimalist customization.
- 👻 Pake is just a simple tool — replace the old bundle approach with Tauri (though PWA is good enough).


## Before starting

1. **For beginners**: Play with Popular Packages to find out Pake's capabilities, or try to pack your application with [GitHub Actions](<https://github.com/tw93/Pake/wiki/Online-Compilation-(used-by-ordinary-users)>). Don't hesitate to reach for assistance at [Discussion](https://github.com/tw93/Pake/discussions)!
2. **For developers**: “Command-Line Packaging” supports macOS fully. For Windows/Linux users, it requires some tinkering. [Configure your environment](https://tauri.app/v1/guides/getting-started/prerequisites) before getting started.
3. **For hackers**: For people who are good at both front-end development and Rust, how about customizing your apps' function more with the following [Customized Development](#development)?

## Command-Line Packaging

![Pake](https://raw.githubusercontent.com/tw93/static/main/pake/pake.gif)

**Pake provides a command line tool, making the flow of package customization quicker and easier. See [documentation](./bin/README.md) for more information.**

```bash
# Install with npm
npm install -g pake-cli

# Command usage
pake url [OPTIONS]...

# Feel free to play with Pake! It might take a while to prepare the environment the first time you launch Pake.
pake https://weekly.tw93.fun --name Weekly --hide-title-bar

```

If you are new to the command line, you can compile packages online with _GitHub Actions_. See the [Tutorial](<https://github.com/tw93/Pake/wiki/Online-Compilation-(used-by-ordinary-users)>) for more information.

## Development

Prepare your environment before starting. Make sure you have Rust `>=1.63` and Node `>=16` (e.g., `16.18.1`) installed on your computer. For installation guidance, see [Tauri documentation](https://tauri.app/v1/guides/getting-started/prerequisites).

If you are unfamiliar with these, it is better to try out the above tool to pack with one click.

```sh
# Install Dependencies
npm i

# Local development [Right-click to open debug mode.]
npm run dev

# Pack application
npm run build
```

## Advanced Usage

1. You can refer to the [codebase structure](https://github.com/tw93/Pake/wiki/Description-of-Pake's-code-structure) before working on Pake, which will help you much in development.
2. Modify the `url` and `productName` fields in the `pake.json` file under the src-tauri directory, the "domain" field in the `tauri.config.json` file needs to be modified synchronously, as well as the `icon` and `identifier` fields in the `tauri.xxx.conf.json` file. You can select an `icon` from the `icons` directory or download one from [macOSicons](https://macosicons.com/#/) to match your product needs.
3. For configurations on window properties, you can modify the `pake.json` file to change the value of `width`, `height`, `fullscreen` (or not), `resizable` (or not) of the `windows` property. To adapt to the immersive header on Mac, change `hideTitleBar` to `true`, look for the `Header` element, and add the `padding-top` property.
4. For advanced usages such as style rewriting, advertisement removal, JS injection, container message communication, and user-defined shortcut keys, see [Advanced Usage of Pake](https://github.com/tw93/Pake/wiki/Advanced-Usage-of-Pake).



## Frequently Asked Questions

1. Right-clicking on an image element in the page to open the menu and select download image or other events does not work (common in MacOS systems). This issue is due to the MacOS built-in webview not supporting this feature.

## Support

1. I have two cats, TangYuan and Coke. If you think Pake delights your life, you can feed them <a href="https://miaoyan.app/cats.html?name=Pake" target="_blank">some canned food 🥩</a>.
2. If you like Pake, you can star it on GitHub. Also, welcome to [recommend Pake](https://twitter.com/intent/tweet?url=https://github.com/tw93/Pake&text=%23Pake%20-%20A%20simple%20Rust%20packaged%20web%20pages%20to%20generate%20Mac%20App%20tool,%20compared%20to%20traditional%20Electron%20package,%20the%20size%20of%20nearly%2040%20times%20smaller,%20generally%20about%202M,%20the%20underlying%20use%20of%20Tauri,%20performance%20experience%20than%20the%20JS%20framework%20is%20much%20lighter~) to your friends.
3. You can follow my [Twitter](https://twitter.com/HiTw93) to get the latest news of Pake or join our [Telegram](https://t.me/+GclQS9ZnxyI2ODQ1) chat group.
4. I hope that you enjoy playing with it. Let us know if you find a website that would be great for a Mac App!