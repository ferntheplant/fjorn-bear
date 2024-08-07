---
title: Yet Another Dotfiles Repo
link: yet-another-dotfiles-repo
published_date: 2024-25-03
meta_description: Color me impressed /s
make_discoverable: false
tags: programming
---

Table of contents

<!-- toc -->

- [Architecture](#architecture)
  * [Directory Layout](#directory-layout)
  * [Scripts](#scripts)
  * [Reproducibility](#reproducibility)
- [Package Management](#package-management)
  * [Mise En Place](#mise-en-place)
  * [The Dreadful Manual Installs](#the-dreadful-manual-installs)
- [Actual Configs](#actual-configs)
  * [Catppuccin Macchiato](#catppuccin-macchiato)
  * [zshrc](#zshrc)
  * [Zellij](#zellij)
  * [Helix](#helix)
    - [Language Server Configuration](#language-server-configuration)
    - [Tree File Picker](#tree-file-picker)
      <br></br>

<!-- tocstop -->

Yeah yeah I know, everybody and their mom has made their own "guide to dotfiles" before. I can't say mine is any different, but it is mine. You can find the repo [here](https://github.com/ferntheplant/dotfiles). Hope you learn a thing or two.

## Architecture

My dotfiles repo is architected around using [`stow`](https://www.gnu.org/software/stow/manual/stow.html), a self-proclaimed "symlink farm manager" with a bare-bones set of features. `stow` has no dependencies, fancy syntax, or even sub-commands - it does exactly one thing, and it does it well. Stow simply takes files from one location and symlinks them somewhere else.

### Directory Layout

```text
/home/fjorn/dotfiles
├── nvim
│  └── .config
│     └── nvim
│        ├── lua
│        │  ├── custom
│        │  │  └── plugins
│        │  └── kickstart
│        │     ├── plugins
│        │     └── health.lua
│        └── init.lua
├── starship
│  └── .config
│     └── starship.toml
└── zsh
   └── .zshrc
```

Starting from the _bottom_ we have our `zsh` directory that contains dotfiles for configuring our shell, `zsh`. By default, when we use `stow` on a directory it symlinks all files within that directory into the current _parent_ directory. So by running `stow zsh` from `~/dotfiles` we create a symlink in the home directory to `.zshrc` in the dotfiles directory.

This trick even works on nested directories like those for `starship` and `nvim`. With the `--no-folding` flag `stow` will create subdirectories like `.config/nvim/lua` and symlink the nested files within the newly created subdirectories. After running `stow --no-folding` on each directory in my dotfiles folder my home directory might look something like this:

```text
/home/fjorn
├── .config
│  ├── nvim
│  │  ├── lua
│  │  │  ├── custom
│  │  │  │  └── plugins
│  │  │  │     └── init.lua *
│  │  │  └── kickstart
│  │  │     ├── plugins
│  │  │     │  ├── debug.lua *
│  │  │     │  ├── indent_line.lua *
│  │  │     │  └── lint.lua *
│  │  │     └── health.lua *
│  │  └── init.lua *
│  └── starship.toml *
├── dotfiles
│  ├── nvim
│  │  └── .config
│  │     └── nvim
│  │        ├── lua
│  │        │  ├── custom
│  │        │  │  └── plugins
│  │        │  │     └── init.lua
│  │        │  └── kickstart
│  │        │     ├── plugins
│  │        │     │  ├── debug.lua
│  │        │     │  ├── indent_line.lua
│  │        │     │  └── lint.lua
│  │        │     └── health.lua
│  │        └── init.lua
│  ├── starship
│  │  └── .config
│  │     └── starship.toml
│  └── zsh
│     └── .zshrc
└── .zshrc *
```

The starred files are symlinks to their respective locations in the `~/dotfiles` directory. Generally most tools will look for configuration in `~/.config/<tool name>` so being able to quickly establish symlinks with the correct nesting structure makes configuring these tools super easy.

### Scripts

Symlinking config files to `~/.config` is useful, but how can I get custom scripts I wrote moved to a reasonable location (like `~/.local/bin`) so they can be found on my `$PATH`? The solution is to use the `stow --target` flag to specify a directory other than the current parent to symlink files into. I put all my custom scripts (with correct executable permissions - `chmod +x <script name>`) in the `~/dotfiles/scripts` directory. Then instead of using the `stow --no-folding` command I use on my other dotfile subdirectories I use `stow --target='/home/fjorn/.local/bin' scripts` to symlink every script to my local bin folder.

To automate this I wrote a custom install script you can find [here](https://github.com/ferntheplant/dotfiles/blob/main/scripts/install) that calls `stow --no-folding` on every subdirectory of my dotfiles except for `scripts` where it calls `stow --target='...'` like above.

### Reproducibility

The primary goal of this entire setup is reproducibility. I want to be able to set up a whole new machine with all my favorite tools and configurations with minimal effort. The [`README.md`](https://github.com/ferntheplant/dotfiles/blob/main/README.md) outlines the specific procedures for leaving one machine and setting up a fresh machine with as few commands as possible.

## Package Management

My main operating systems are macOS and Ubuntu on WSL on Windows. With Ubuntu, I can use the default `apt` package manager and on macOS I can use `homebrew`. However, both managers often miss critical tools I use, so I need alternative packaging systems for acquiring those tools in a reproducible way.

### Mise En Place

[`mise-en-place`](https://mise.jdx.dev/) is a "dev env" manager that installs and manages versions of common dev tools like go, cargo, and bun. Most tools I want to use that aren't available on my OS default package managers can usually be found through these language-specific managers - especially cargo. So I use `mise` to install these language specific tools and then use a `leave-<tool>` pattern (see [#zshrc](#zshrc)) to install all my packages for each tool. See the [`README`](https://github.com/ferntheplant/dotfiles/blob/main/README.md) for more info.

### The Dreadful Manual Installs

Despite the optionality provided by my OS package manager and my dev tool package managers there still exist tools that I must install manually. I haven't found a good way to automate installing these tools, so I've defaulted to simply including the installation instructions as part of my own `README` for me to manually execute when setting up a new machine. If you have any good recommendations for managing these "custom" installations please [let me know!](mailto:me@fjorn.dev)

## Actual Configs

Now for the real meat of any dotfiles repo: the actual config files. I've included a walkthrough of my most "steal-able" configs below. Enjoy!

### Catppuccin Macchiato

Wherever possible I like to use [Catppuccin](https://catppuccin.com/) themes to give my terminal a coherent aesthetic. So far the only tools that have a catppuccin theme I could find are:

- Alacritty
- Helix
- Yazi
- Zellij

### zshrc

Most stuff in my `.zshrc` is basic post-install setup for various tools directly pasted or `echo`-ed in from the tools' instructions. The main takeaways are:

- Alias `l` to a nice `exa` command for a solid tree view of any directory
  * Can easily override `--level=1` if the nesting is overwhelming
- Adding `/home/fjorn/.local/bin` to my `$PATH` for easy access to custom scripts or binaries I build myself
- The `leave-<tool>` pattern dumps any packages installed with `<tool>` into a plain text file with each package name delimited by spaces
  * To re-install these packages simply pipe the file contents into the `<tool>` install command
  * The parsing and formatting of package manager outputs using `jq`, `awk`, `grep`, etc. was all done by ChatGPT so ask it for help doing something similar for other package managers

### Zellij

I'm too cool for the usual terminal tools other techies use and so instead of [tmux](https://github.com/tmux/tmux/wiki) I use [zellij](https://zellij.dev/) to manage my terminal workspaces. Zellij is basically tmux written in rust with a nicer visual UX for navigating panes, tabs, and sessions. I actually didn't customize my zellij config much at all but wanted to include it here as it's one of my primary terminal tools.

### Helix

Once again, I'm apparently too cool for vim/nvim and way too cool for VS Code, so I use [Helix](https://helix-editor.com/) as my primary text editor.

Pros:

- "motion → command" syntax makes way more sense to me than the default vim "command → motion"
  * Ex: in vim to delete the currently hovered word you press `dw` to say "delete word" but in Helix the command is `wd` which reads as "select word then delete selection"
- Written in Rust™ so it's "blazingly fast"
- Sane defaults and built-in features so no plugins and minimal configuration are needed
- "Unix philosophy" around using existing shell tools to manipulate text instead of writing wrappers for them as plugins

Cons:

- No plugin system yet means you have to hack together scripts to do stuff you could get with nvim plugins
- Saying "I use Helix btw" doesn't sound as cool as saying I use vim
- No up-to-date packages on `apt`, so I had to install via some random guy's PPA

#### [Language Server Configuration](#language-server-configuration)

Since Helix has no plugin system you have to manually install language servers for languages you want to use. Luckily Helix does come with sane configurations built-in for common language servers so all you have to do is install the server. See their [docs on default language servers](https://github.com/helix-editor/helix/wiki/How-to-install-the-default-language-servers)

The only real changes to the default I made were to use [biome](https://biomejs.dev/) as a language server and to use [dprint](https://dprint.dev/) as my formatter for many languages.

#### [Tree File Picker](#tree-file-picker)

The most common complaint with Helix is that there is no tree-style file picker like in VS Code. The default file picker exclusively uses fuzzy finding to locate files through text search rather than visual hierarchical exploration with a tree like many coders are used to. The Helix core team closed a PR adding such a tree file picker as they say it would be better implemented as plugin, but as mentioned earlier the plugin system is not live so neither is a tree file picker.

I hacked one together using some funny scripts with `zellij`. You can read how I did it on [this GitHub discussion](https://github.com/helix-editor/helix/discussions/8314#discussioncomment-8921134).
