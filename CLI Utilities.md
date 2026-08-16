# CLI Utilities

## Overview

My Fedora desktop uses **zsh** as the primary shell along with a curated collection of modern command-line utilities. The goal is not to replace every standard Unix utility, but to augment the command line with tools that improve everyday workflows while remaining simple, understandable, and portable.

Every utility in this document has earned its place through regular use.

---

## Philosophy

- Learn modern tools instead of hiding them behind aliases.
- Keep the shell configuration clean and understandable.
- Install only tools that solve real problems.
- Keep standard Unix commands available.
- Use aliases sparingly.
- Build a toolkit that can be recreated easily on any Fedora installation.

For example, I intentionally use `rg` instead of aliasing `grep`, and `fd` instead of aliasing `find`.

---

# Shell

The default shell is **zsh**.

Current shell environment:

- Oh My Zsh
- Powerlevel10k
- git plugin
- zsh-autosuggestions
- zsh-syntax-highlighting

Neovim is configured as the default editor.

```zsh
export EDITOR="nvim"
export VISUAL="nvim"
```

---

# Installation

```bash
sudo dnf install \
    eza \
    bat \
    fd-find \
    ripgrep \
    fzf \
    zoxide \
    btop \
    fastfetch \
    jq \
    yq \
    tree \
    curl \
    wget \
    rsync
```

If Fedora package downloads fail because of stale mirrors:

```bash
sudo dnf clean all
sudo dnf makecache --refresh
sudo dnf --refresh install <package>
```

---

# CLI Toolkit

## eza

Modern replacement for `ls`.

```bash
eza
eza -la
eza -la --git
eza --tree --level=2
```

## wl-clipboard

Command-line access to the Wayland clipboard.

- `wl-copy` — copy data to the clipboard
- `wl-paste` — output clipboard contents

```bash
ls -la | wl-copy
pwd | wl-copy
cat ~/.zshrc | wl-copy
wl-paste
```

This replaces `xclip` and `xsel` when using Wayland.

## bat

Modern file viewer designed for reading files, with syntax highlighting, line numbers, Git integration, and paging through `less`.

```bash
bat ~/.zshrc
bat README.md
bat ~/.config/hypr/hyprland.conf
```

Use `cat` when streaming or piping file contents.

## fd

Modern file finder.

```bash
fd docker
fd README
fd toml
fd zsh
```

## ripgrep (`rg`)

Fast recursive text search.

```bash
rg TODO
rg alias ~/.zshrc
rg "Nextcloud"
rg "server_name"
```

## fzf

Interactive fuzzy finder useful for command history, files, directories, Git branches, and search results.

```bash
history | fzf
```

## zoxide

Smart directory navigation that learns frequently used directories.

```zsh
eval "$(zoxide init zsh)"
```

```bash
z projects
z downloads
z config
```

## btop

Modern terminal system monitor.

```bash
btop
```

## fastfetch

Displays system and environment information.

```bash
fastfetch
```

## jq

Command-line JSON processor.

```bash
echo '{"name":"Gary","os":"Fedora"}' | jq
```

## yq

YAML processor.

```bash
echo 'name: Gary' | yq
```

## tree

Displays directory structures visually.

```bash
tree
tree -L 2
tree -L 3
tree -L 2 -I node_modules
```

## curl

Command-line HTTP client.

```bash
curl https://example.com
curl -I https://example.com
```

## wget

Simple file downloader.

```bash
wget https://example.com/file.zip
```

## rsync

Efficient file synchronization utility.

```bash
rsync -avh source/ destination/
```

---

# Reading and Inspecting Text

## `less` — Terminal Pager

`less` is a terminal pager used to view text one screen at a time without opening it in an editor.

Many CLI tools automatically send long output through a pager. Git commonly does this for commands such as:

```bash
git diff
git log
git show
```

When Git output becomes scrollable and pressing `q` returns to the shell, the output is usually being displayed through `less`.

### Navigation

Many `less` navigation commands overlap with Vim.

| Key | Action |
| --- | --- |
| `j` / `↓` | Down one line |
| `k` / `↑` | Up one line |
| `Space` / `Ctrl-f` | Forward one screen |
| `b` / `Ctrl-b` | Back one screen |
| `d` / `Ctrl-d` | Down half a screen |
| `u` / `Ctrl-u` | Up half a screen |
| `g` | Beginning |
| `G` | End |
| `/text` | Search forward |
| `?text` | Search backward |
| `n` | Next search match |
| `N` | Previous search match |
| `q` | Quit |

Search forward:

```text
/error
```

Search backward:

```text
?error
```

Press `n` for the next match and `N` for the previous match.

### Using `less` Directly

```bash
less filename.txt
some-command | less
```

### Git and the Pager

Bypass Git's pager for one command:

```bash
git --no-pager diff
```

This is especially useful when the output is intended for another command:

```bash
git --no-pager diff | wl-copy
```

A pager can also be disabled for a specific Git command:

```bash
git config --global pager.diff false
```

Generally, keeping paging enabled for interactive terminal use and using `--no-pager` when needed is the better default.

---

## `sed` Basics

`sed` (Stream Editor) can inspect or modify text streams. For quick inspection, it is useful for printing a specific range of lines without opening an editor.

```bash
sed -n '1,20p' file.php
sed -n '100,150p' file.php
sed -n '250,300p' file.php
```

`-n` suppresses normal output and `p` prints the requested lines.

Overshooting the end of a file is harmless:

```bash
sed -n '1,200p' ReviewItemResource.php
```

If the file contains only 72 lines, `sed` prints those 72 lines and exits.

## `head`

```bash
head file.php
head -30 file.php
```

## `tail`

```bash
tail file.php
tail -50 file.php
```

## `wc`

Count lines:

```bash
wc -l file.php
```

## Typical Inspection Workflow

```bash
rg "translation" app/Http/Resources/ReviewItemResource.php
wc -l app/Http/Resources/ReviewItemResource.php
sed -n '1,120p' app/Http/Resources/ReviewItemResource.php
sed -n '40,80p' app/Http/Resources/ReviewItemResource.php
```

The goal is not to memorize every `sed` feature. The goal is to answer questions quickly without leaving the terminal.

### Quick Reference

| Command | Description |
| --- | --- |
| `less file` | Page through a file |
| `sed -n '1,20p' file` | Print lines 1–20 |
| `sed -n '100,150p' file` | Print lines 100–150 |
| `head file` | First 10 lines |
| `head -30 file` | First 30 lines |
| `tail file` | Last 10 lines |
| `tail -50 file` | Last 50 lines |
| `wc -l file` | Count lines |
| `rg "text"` | Search recursively for text |

---

# Shell Features

## Brace Expansion

```bash
mkdir -p app/pages/{capture,review,vocabulary}
touch {a,b,c}.txt
touch app/pages/{capture,review,moments}/index.vue
mv *.{jpg,png} docs/ui/
```

Useful for quickly scaffolding projects without loops.

---

# Aliases

Aliases are intentionally kept to a minimum.

```zsh
alias vim='nvim'

alias ll='eza -lah --icons --git'
alias la='eza -a --icons'
alias lt='eza --tree --level=2 --icons'

alias cls='clear'

alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

alias cp='cp -i'
alias mv='mv -i'
alias rm='rm -i'
```

Intentionally **not** aliased:

- `ls`
- `cat`
- `grep`
- `find`
- `top`
- `cd`

---

# `repos`

Central Git checkpoint for repositories recursively discovered under `~/code` plus explicitly configured repositories that intentionally live elsewhere.

```bash
repos
repos --fetch
repos --help
```

Script:

```text
~/code/dotfiles/bin/repos
```

Current explicit non-code repository:

```text
~/Nextcloud/ObsidianVault/Infrastructure
```

See [Projects](Projects.md).

---

# Daily Workflow

```bash
# Search
rg TODO
rg "Nextcloud"

# Find files
fd docker
fd README

# Read configuration
bat ~/.zshrc

# Browse directories
ll
lt

# Jump between directories
z projects
z config

# Monitor system
btop

# System information
fastfetch

# JSON / YAML
jq '.' data.json
yq '.' compose.yml

# Directory structure
tree -L 2

# Copy a complete Git diff without paging
git --no-pager diff | wl-copy
```

---

# Maintenance

```bash
sudo dnf upgrade
```

If package installation fails due to stale mirrors:

```bash
sudo dnf clean all
sudo dnf makecache --refresh
sudo dnf --refresh install <package>
```

---

# Related Documentation

- [Fedora Desktop](Fedora%20Desktop.md)
- [Neovim](Neovim.md)
- [Hyprland](Hyprland.md)
- [Tmux](Tmux.md)
- [Git](Git.md)
- [SSH Troubleshooting](SSH%20Troubleshooting.md)
