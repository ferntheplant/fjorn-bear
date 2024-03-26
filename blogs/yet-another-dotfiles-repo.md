---
title: Yet Another Dotfiles Repo
link: yet-another-dotfiles-repo
published_date: TODO
meta_description: Color me impress /s
make_discoverable: false
---

Yeah yeah I know, everybody and their mom has made their own "guide to dotfiles" before. I can't say mine is any different, but it is mine. Hope you enjoy.

## Architecture

My dotfiles repo is architected around using [`stow`](https://www.gnu.org/software/stow/manual/stow.html), a self-proclaimed "symlink farm manager" with a bare-bones set of features. `stow` has no dependencies, fancy syntax, or even sub-commands - it does exactly one thing, and it does it well. Stow simply takes files from one location and symlinks them somewhere else.

### Directory Layout

```
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

Starting from the _bottom_ we have our `zsh` directory that contains dotfiles for configuring our shell, `zsh`. By default, when we use `stow` stow on a directory it symlinks all files within that directory into the current _parent_ directory. So by running `stow zsh` from `~/dotfiles` we create a symlink in the home directory to `.zshrc` in the dotfiles directory.

This trick even works on nested directories like those for `starship` and `nvim`. With the `--no-folding` flag

```javascript
const a = (x) => 10;
```

### Reproducibility

## Package Management

### Mise En Place

### Language-Specific Package Managers

### The Dreadful Manual Installs

## Actual Configfiles

### Starship

### .zshrc

### Helix

### Zellij
