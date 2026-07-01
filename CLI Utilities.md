# CLI Utilities

## Overview

My Fedora desktop uses **zsh** as the primary shell along with a curated collection of modern command-line utilities. The goal is not to replace every standard Unix utility, but to augment the command line with tools that improve everyday workflows while remaining simple, understandable, and portable.

Every utility in this document has earned its place through regular use.

---

## Philosophy

The command-line environment follows a few simple principles:

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

Install the standard CLI toolkit with:

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

Features:

- Better formatting
- Icons
- Git status
- Tree views
- Improved readability

Examples:

```bash
eza
eza -la
eza -la --git
eza --tree --level=2
```

---

## wl-clipboard

Provides command-line access to the Wayland clipboard.

The two commands are:

- `wl-copy` — Copy data to the clipboard.
- `wl-paste` — Paste data from the clipboard.

This is especially useful when piping the output of commands into the clipboard.

Examples:

```bash
# Copy the output of a command
ls -la | wl-copy

# Copy the current working directory
pwd | wl-copy

# Copy a file to the clipboard
cat ~/.zshrc | wl-copy

# View clipboard contents
wl-paste
```

This utility replaces `xclip` and `xsel` when using Wayland.

---

## bat

Modern file viewer.

Unlike `cat`, `bat` is designed for **reading** files.

Features:

- Syntax highlighting
- Line numbers
- Git integration
- Paging through `less`

Examples:

```bash
bat ~/.zshrc
bat README.md
bat ~/.config/hypr/hyprland.conf
```

Use `cat` when streaming or piping file contents.

---

## fd

Modern file finder.

Simpler and faster than `find`.

Examples:

```bash
fd docker
fd README
fd toml
fd zsh
```

---

## ripgrep (`rg`)

Fast recursive text search.

Probably the most useful CLI tool for software development.

Examples:

```bash
rg TODO
rg alias ~/.zshrc
rg "Nextcloud"
rg "server_name"
```

---

## fzf

Interactive fuzzy finder.

Useful for:

- Command history
- Files
- Directories
- Git branches
- Search results

Example:

```bash
history | fzf
```

More shell integration may be added in the future.

---

## zoxide

Smart directory navigation.

Rather than replacing `cd`, `zoxide` quietly learns which directories are used most often.

Initialize in `.zshrc`:

```zsh
eval "$(zoxide init zsh)"
```

Examples:

```bash
z projects
z downloads
z config
```

The more it is used, the smarter it becomes.

---

## btop

Modern terminal system monitor.

Displays:

- CPU usage
- Memory
- Network
- Disk activity
- Running processes

Example:

```bash
btop
```

---

## fastfetch

Displays system information.

Useful for quickly viewing:

- Distribution
- Kernel
- Desktop environment
- Shell
- Hardware
- Memory
- Terminal

Example:

```bash
fastfetch
```

---

## jq

Command-line JSON processor.

Useful when working with:

- APIs
- Docker
- GitHub
- Configuration files

Example:

```bash
echo '{"name":"Gary","os":"Fedora"}' | jq
```

---

## yq

YAML equivalent of `jq`.

Useful for:

- Docker Compose
- Kubernetes
- Ansible
- Configuration files

Example:

```bash
echo 'name: Gary' | yq
```

---

## tree

Displays directory structures visually.

Examples:

```bash
tree
tree -L 2
tree -L 3
```

---

## curl

Command-line HTTP client.

Useful for:

- APIs
- Downloads
- Testing endpoints

Examples:

```bash
curl https://example.com
curl -I https://example.com
```

---

## wget

Simple file downloader.

Example:

```bash
wget https://example.com/file.zip
```

---

## rsync

Efficient file synchronization utility.

Useful for:

- Backups
- Copying directories
- Synchronization

Example:

```bash
rsync -avh source/ destination/
```

---

# Aliases

Aliases are intentionally kept to a minimum.

Rather than replacing modern tools with old command names, the goal is to learn the actual commands.

Current aliases:

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

These commands are left separate so the actual tools become second nature.

---

# Daily Workflow

Search for text:

```bash
rg TODO
rg "Nextcloud"
```

Find files:

```bash
fd docker
fd README
```

Read configuration files:

```bash
bat ~/.zshrc
```

Browse directories:

```bash
ll
lt
```

Jump between common directories:

```bash
z projects
z config
```

Monitor the system:

```bash
btop
```

View system information:

```bash
fastfetch
```

Process JSON:

```bash
cat data.json | jq
```

Process YAML:

```bash
cat compose.yml | yq
```

View a project's directory structure:

```bash
tree -L 2
```

---

# Maintenance

CLI utilities are updated through Fedora's package manager.

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

- [[Fedora Desktop]]
- [[Neovim]]
- [[Hyprland]]
- tmux *(Coming Soon)*
- Git *(Coming Soon)* 
- SSH *(Coming Soon)*
- [[TODO]]