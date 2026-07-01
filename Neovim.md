# Neovim

## Purpose

This page documents my Neovim environment.

The goal is not to document Vim itself, but rather how my editor is configured, how I use it, and the things I regularly forget.

The configuration files are the implementation. This page documents the design decisions, workflow, and anything Future Gary is likely to forget.

---

## Configuration Files

| File | Purpose |
|------|---------|
| `~/.config/nvim/init.lua` | Entry point |
| `~/.config/nvim/lua/config/` | Core LazyVim configuration |
| `~/.config/nvim/lua/plugins/` | Plugin configuration |
| `lazy-lock.json` | Locked plugin versions |

---

## Installation

- Neovim
- LazyVim

---

## Editor Philosophy

- Stay close to upstream LazyVim.
- Customize only after identifying real friction.
- Learn Vim rather than working around it.
- Prefer keyboard-first workflows.
- Keep the configuration simple and understandable.

---

## Core Features

### LazyVim

Provides the overall editor experience.

### Telescope

Primary file and text search.

### Neo-tree

Project explorer.

### Harpoon

Quick navigation between frequently used files.

### LSP

Code intelligence and diagnostics.

### Treesitter

Syntax parsing and highlighting.

### Mason

Language server and tooling management.

### Git

Integrated Git support through LazyVim.

---

## Custom Keybindings

### Clipboard

| Shortcut    | Purpose                                                                      |
| ----------- | ---------------------------------------------------------------------------- |
| `<leader>p` | Paste from the system clipboard below the cursor and automatically re-indent |

### Editing

| Shortcut    | Purpose                 |
| ----------- | ----------------------- |
| `<leader>o` | Insert blank line below |
| `<leader>O` | Insert blank line above |

### Navigation

The following commands automatically keep the cursor centered while navigating:

- `Ctrl-d`
- `Ctrl-u`
- `n`
- `N`
- `*`
- `#`

---

## Telescope

Frequently used shortcuts:

| Shortcut     | Purpose    |
| ------------ | ---------- |
| `<leader>fd` | Find files |
| `<leader>fg` | Live grep  |

---

## Daily Workflow

Typical editing session:

- Open project.
- Find files with Telescope.
- Navigate between important files with Harpoon.
- Use LSP features while editing.
- Search with Telescope instead of manually browsing directories.
- Commit changes with Git.

---

## Things Future Gary Will Forget

### Motions

(To be filled in.)

---

### Operators

(To be filled in.)

---

### Text Objects

(To be filled in.)

---

### Telescope

(To be filled in.)

---

### Harpoon

(To be filled in.)

---

### Git

(To be filled in.)

---

### LSP

(To be filled in.)

---

### Macros

(To be filled in.)

---

### Commands I Rarely Use

(To be filled in.)

---

## Personal Preferences

- Two-space indentation.
- Space is the leader key.
- Snacks animations are disabled.

---

## Related

- [[Fedora Desktop]]
- [[Hyprland]]
- [[tmux]]
- [[Git]]