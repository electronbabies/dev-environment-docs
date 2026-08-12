# Neovim

## Purpose

This page documents my Neovim environment.

The goal is not to document Vim itself, but rather how my editor is configured, how I use it, and the things I regularly forget.

The configuration files are the implementation. This page documents the design decisions, workflow, and anything Future Gary is likely to forget.

---

## Configuration Files

| File                          | Purpose                    |
| ----------------------------- | -------------------------- |
| `~/.config/nvim/init.lua`     | Entry point                |
| `~/.config/nvim/lua/config/`  | Core LazyVim configuration |
| `~/.config/nvim/lua/plugins/` | Plugin configuration       |
| `lazy-lock.json`              | Locked plugin versions     |

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

## Leader Key

Leader: `Space`

Press `Space` and pause to open the LazyVim command menu.

### Clipboard

| Shortcut    | Purpose                                                                      |
| ----------- | ---------------------------------------------------------------------------- |
| `<leader>p` | Paste from the system clipboard below the cursor and automatically re-indent |

When replacing visually selected text, normal `p` replaces the selection but also puts the replaced text into Vim's default register. That can be surprising when repeatedly pasting the same block.

The remaining friction in the current ChatGPT → Neovim workflow is moving code between the browser and editor. Solve that with a keyboard-first clipboard or CLI workflow rather than by changing editors.

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

- Open the project inside its tmux session.
- Find files by filename with Telescope instead of browsing a file tree.
- Navigate between important files with Harpoon.
- Use text objects and motions for structural edits.
- Use LSP features while editing.
- Search project text with Telescope/ripgrep.
- Keep related application repositories in separate tmux windows when needed.
- Commit changes with Git.

The Neovim/tmux workflow is now comfortable enough for normal project work. It has not introduced noticeable slowdown compared with the previous JetBrains workflow. The main remaining interruption is browser-to-editor copying and pasting.

---

## Things Future Gary Will Forget

### Motions

(To be filled in.)

---

### Operators

(To be filled in.)

---

### Text Objects

Text objects are now part of the normal editing workflow.

| Command | Purpose |
| --- | --- |
| `ci{` | Change inside curly braces |
| `ca{` | Change around curly braces |
| `ci(` | Change inside parentheses |
| `ci[` | Change inside square brackets |
| `ci"` | Change inside double quotes |
| `ci'` | Change inside single quotes |
| `cit` | Change inside an HTML/XML tag |
| `cat` | Change around an HTML/XML tag |
| `dat` | Delete around an HTML/XML tag |

Mental model:

- If the cursor is already inside the object, operate on the object directly.
- If it is not, navigate to the object first.
- `%` is useful for jumping between matching delimiters.
- `$%` is a fast pattern when the end of a line contains the opening delimiter for the block I want.

See also: [[Vim Notes]]

---

### Telescope

- Prefer filename search over manually browsing a file tree.
- `<leader>fd` finds files.
- `<leader>fg` searches project text.
- Neo-tree is available, but it is not the preferred navigation model.

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
- Prefer searching for files by filename instead of navigating a file tree.
- Prefer structural Vim edits (`ci{`, `cit`, etc.) over mouse selection.
- Keep the environment reproducible and document workflow changes as they become permanent.

---

## Related

- [[Fedora Desktop]]
- [[Hyprland]]
- [[Tmux]]
- [[Git]]