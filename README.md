# pi-launcher

A small bash script that pops up [pi](https://github.com/earendil-works/pi) in
a tmux session. Each directory gets its own pi session, so hitting the key again
from the same project drops you right back where you left off.

## Requirements

- bash
- tmux 3.2 or later (needed for `display-popup -E`)
- pi on your `PATH`
- macOS (the script uses `md5 -q`)

## Install

Clone the repo somewhere stable:

```sh
git clone https://github.com/ryanjayleyva/pi-launcher ~/path/to/pi-launcher
```

Then bind it to a key in tmux (see below).

## How it works

1. Grabs the current pane's working directory.
2. Hashes the path to build a session name like `pi-1a2b3c4d`.
3. Creates a detached tmux session running `pi` in that directory, if one
   doesn't already exist.
4. Opens an 80% x 80% popup attached to that session.

Close the popup by detaching (`C-a d`). pi keeps running in the
background session, and the next launch from that directory reattaches to it.

## Bind it to a key

The script has to run from inside tmux, so a key binding is the way to go. Add
this line to your tmux config:

```tmux
bind y run-shell "~/path/to/pi-launcher/pi-launcher.sh"
```

Now `C-a y` opens pi. Swap `y` for whatever key you like.

### Where does the line go?

Most dev keep everything in `~/.tmux.conf` (or `~/.config/tmux/tmux.conf`).
Just paste the line at the bottom of that file.

Mine is modular. `tmux.conf` does nothing but source other files:

```tmux
source-file ~/.config/tmux/options.conf
source-file ~/.config/tmux/terminal.conf
source-file ~/.config/tmux/bindings.conf
source-file ~/.config/tmux/statusline.conf
```

So my binding lives in `bindings.conf`:

```tmux
# Launch pi
bind y run-shell "~/dotfiles-macos/scripts/pi-launcher.sh"
```

If you split yours up too, put it with your other bindings. Same line, different file.

### Reload

```sh
tmux source-file ~/.tmux.conf
```

Point that at whichever file is your entry point. Or just restart tmux.

## Usage

From any tmux pane, press `C-a y`. If you haven't changed your prefix from
the tmux default, that's `C-b y` instead.

You can also run the script directly, but only from inside a tmux pane:

```sh
./pi-launcher.sh
```

## License

[MIT](LICENSE)
