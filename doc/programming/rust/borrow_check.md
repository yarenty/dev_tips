# RustOWL

Visualize ownership and lifetimes in Rust for debugging and optimization. 


https://github.com/cordx56/rustowl


![](https://github.com/cordx56/rustowl/raw/main/docs/readme-screenshot.png)


RustOwl visualizes ownership movement and lifetimes of variables. When you save Rust source code, it is analyzed, and the ownership and lifetimes of variables are visualized when you hover over a variable or function call.

RustOwl visualizes those by using underlines:

- 🟩 green: variable's actual lifetime
- 🟦 blue: immutable borrowing
- 🟪 purple: mutable borrowing
- 🟧 orange: value moved / function call
- 🟥 red: lifetime error - diff of lifetime between actual and expected


Currently, we offer VSCode extension, Neovim plugin and Emacs package. For these editors, move the text cursor over the variable or function call you want to inspect and wait for 2 seconds to visualize the information. We implemented LSP server cargo owlsp with an extended protocol. So, RustOwl can be used easily from other editor.

