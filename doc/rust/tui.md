# Cursive

A TUI (Text User Interface) library focused on ease-of-use.

https://crates.io/crates/cursive

Cursive is a TUI (Text User Interface) library for rust. It uses ncurses by default, but other backends are available.

It allows you to build rich user interfaces for terminal applications.



## Grin
Quite good tui - check how is implemented!
https://github.com/mimblewimble/grin


## Cursive modules


## Table view
A basic table view implementation for cursive.

https://github.com/BonsaiDen/cursive_table_view

https://crates.io/crates/cursive_table_view


## Tree view

A basic tree view implementation for cursive.

https://github.com/BonsaiDen/cursive_tree_view
https://crates.io/crates/cursive_tree_view




## Multiple tabs
This project provides a wrapper view to be able to easily handle multiple tabs that can be switched to at any time without having to change the order of the views for gyscos/cursive views.


https://github.com/deinstapel/cursive-tabs


## Spinner view

spinning world , etc...

See in examples folder To see in action run cargo run --example spinner

https://github.com/otov4its/cursive-spinner-view/

https://crates.io/crates/cursive-spinner-view


## Logger view
Auto updates from file

This project provides a new debug view for gyscos/cursive using the emabee/flexi_logger crate. This enables the FlexiLoggerView to respect the RUST_LOG environment variable as well as the flexi_logger configuration file. Have a look at the demo below to see how it looks.

https://github.com/deinstapel/cursive-flexi-logger-view

https://crates.io/crates/cursive-flexi-logger-view


## Multiplex (tmux)

This project provides a tiling window manager for gyscos/cursive similar to Tmux. You can place any other cursive view inside of a Mux view to display these views in complex layouts side by side. Watch the demo below to see how it looks.

https://github.com/deinstapel/cursive-multiplex

https://crates.io/crates/cursive-multiplex


## Async veiw -- progress bar

This project provides a wrapper view with a loading screen for gyscos/cursive views. The loading screen will disappear once the wrapped view is fully loaded. This is useful for displaying views which may take long to construct or depend on e.g. the network.


https://github.com/deinstapel/cursive-async-view



## Align view
right-top , middle-top.... left-bottom,

This project provides an AlignedView for gyscos/cursive views which makes it possible to align the child view (center, left, right, top, bottom). The AlignedView uses the required_size reported by the child view and fills the rest of the available space with the views background color.


https://github.com/deinstapel/cursive-aligned-view


## Hexview

A simple and basic hexviewer which can be used with cursive.

https://github.com/hellow554/cursive_hexview


## Markup

The cursive-markup crate provides a markup view for cursive that can render HTML.

https://crates.io/crates/cursive-markup

https://git.sr.ht/~ireas/cursive-markup-rs



# Termion

https://gitlab.redox-os.org/redox-os/termion


Termion is a pure Rust, bindless library for low-level handling, manipulating
and reading information about terminals. This provides a full-featured
alternative to Termbox.
Termion aims to be simple and yet expressive. It is bindless, meaning that it
is not a front-end to some other library (e.g., ncurses or termbox), but a
standalone library directly talking to the TTY.
Termion is quite convenient, due to its complete coverage of essential TTY
features, providing one consistent API. Termion is rather low-level containing
only abstraction aligned with what actually happens behind the scenes. For
something more high-level, refer to inquirer-rs, which uses Termion as backend.
Termion generates escapes and API calls for the user. This makes it a whole lot
cleaner to use escapes.
Supports Redox, Mac OS X, BSD, and Linux (or, in general, ANSI terminals).


# bottom

NICE TEXT GRAPHS

https://crates.io/crates/bottom
