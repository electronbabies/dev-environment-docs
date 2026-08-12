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
tree -L 2 -I node_modules
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

# `sed` Basics

`sed` (Stream Editor) is a command-line tool for reading and modifying text streams. It is most commonly used to inspect or edit files directly from the terminal without opening an editor.

---

#### General Syntax

```bash
sed [options] 'command' file
```

Example:

```bash
sed -n '1,20p' file.php
```

Prints lines **1 through 20**.

---

#### Understanding the Command

```bash
sed -n '1,20p' file.php
```

Breakdown:

| Part       | Meaning                                        |
| ---------- | ---------------------------------------------- |
| `sed`      | Run the stream editor                          |
| `-n`       | Don't print anything unless explicitly told to |
| `'1,20p'`  | Print (`p`) lines 1 through 20                 |
| `file.php` | File to read                                   |

---

#### Common Options

## `-n`

Suppresses normal output.

Without `-n`, `sed` prints every line by default.

With `-n`, only the lines requested with `p` are printed.

---

#### Print a Range of Lines

Print the first 20 lines:

```bash
sed -n '1,20p' file.php
```

Print lines 100–150:

```bash
sed -n '100,150p' file.php
```

Print lines 250–300:

```bash
sed -n '250,300p' file.php
```

If the file ends before the ending line, `sed` simply stops printing.

---

#### How Do You Know Which Range?

Usually...

You don't.

Most developers simply overshoot.

Example:

```bash
sed -n '1,200p' ReviewItemResource.php
```

If the file only has 72 lines, `sed` prints 72 lines and exits.

No error.

---

If you want to know exactly how many lines are in a file:

```bash
wc -l ReviewItemResource.php
```

Example output:

```text
87 ReviewItemResource.php
```

Now you know the file has 87 lines.

---

## `head`

View the beginning of a file.

```bash
head file.php
```

First 10 lines.

```bash
head -30 file.php
```

First 30 lines.

---

## `tail`

View the end of a file.

```bash
tail file.php
```

Last 10 lines.

```bash
tail -50 file.php
```

Last 50 lines.

---

## `wc`

Count lines.

```bash
wc -l file.php
```

Example:

```text
87 file.php
```

---

## `rg` (ripgrep)

Search a project.

```bash
rg "ReviewItemResource"
```

Search for a function:

```bash
rg "updateReviewItem"
```

---

# Typical Workflow

Rather than immediately opening Vim:

```bash
# How big is the file?
wc -l app/Http/Resources/ReviewItemResource.php

# View the beginning
sed -n '1,120p' app/Http/Resources/ReviewItemResource.php

# Find something interesting
rg "translation" app/Http/Resources/ReviewItemResource.php

# Inspect just that section
sed -n '40,80p' app/Http/Resources/ReviewItemResource.php
```

This lets you answer questions quickly while staying entirely in the terminal.

---

# Quick Reference

| Command                  | Description                  |
| ------------------------ | ---------------------------- |
| `sed -n '1,20p' file`    | Print lines 1–20             |
| `sed -n '100,150p' file` | Print lines 100–150          |
| `head file`              | First 10 lines               |
| `head -30 file`          | First 30 lines               |
| `tail file`              | Last 10 lines                |
| `tail -50 file`          | Last 50 lines                |
| `wc -l file`             | Count the number of lines    |
| `rg "text"`              | Search for text in a project |

---

# Philosophy

The goal isn't to memorize every `sed` feature.

The goal is to answer questions quickly without leaving the terminal.

A common workflow is:

```bash
rg "ReviewItemResource"
wc -l app/Http/Resources/ReviewItemResource.php
sed -n '1,200p' app/Http/Resources/ReviewItemResource.php
```

This approach is fast, efficient, and becomes second nature once you spend enough time working from the command line.

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

## Brace Expansion

Create multiple directories:

```bash
mkdir -p app/pages/{capture,review,vocabulary}
```

Create multiple files:

```bash
touch {a,b,c}.txt
```

Create multiple index pages:

```bash
touch app/pages/{capture,review,moments}/index.vue
```

Expand multiple extensions:

```bash
mv *.{jpg,png} docs/ui/
```

Useful for quickly scaffolding projects without loops.

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
- [[tmux]] 
- Git *(Coming Soon)* 
- SSH *(Coming Soon)*