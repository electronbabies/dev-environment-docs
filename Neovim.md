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
| `~/.config/nvim/lua/plugins/colorscheme.lua` | Moonlit Night / Tokyo Night Moon overrides |
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


## Theme

Neovim uses **Tokyo Night Moon** as the underlying colorscheme engine.

The active colorscheme is:

```text
tokyonight-moon
```

Moonlit Night customizations live in:

```text
~/.config/nvim/lua/plugins/colorscheme.lua
```

The goal is not to force the desktop palette onto every syntax group. Code needs stronger semantic differentiation than general desktop UI.

Current visual roles include:

- deep navy background
- soft cool-white normal text
- violet keywords
- cyan functions and methods
- icy blue types
- green strings
- lantern-gold numbers and booleans
- orange constants and special values
- muted blue-gray comments
- crimson diagnostics and errors

Readability takes priority over strict theme uniformity.

Tokyo Night continues to provide the underlying highlight-group behavior; Moonlit Night overrides the palette and selected Treesitter/highlight groups.

See [Moonlit Night](Themes/Moonlit%20Night.md).

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

| Command | Purpose                       |
| ------- | ----------------------------- |
| `ci{`   | Change inside curly braces    |
| `ca{`   | Change around curly braces    |
| `ci(`   | Change inside parentheses     |
| `ci[`   | Change inside square brackets |
| `ci"`   | Change inside double quotes   |
| `ci'`   | Change inside single quotes   |
| `cit`   | Change inside an HTML/XML tag |
| `cat`   | Change around an HTML/XML tag |
| `dat`   | Delete around an HTML/XML tag |

Mental model:

- If the cursor is already inside the object, operate on the object directly.
- If it is not, navigate to the object first.
- `%` is useful for jumping between matching delimiters.
- `$%` is a fast pattern when the end of a line contains the opening delimiter for the block I want.

See also: [Vim Notes](Vim%20Notes.md)

---

### Registers

Vim registers are clipboard/history storage. Yanks, deletes, and changes can all affect registers, so the important thing is knowing where text went and how to retrieve it.

#### Inspecting Registers

Use:

```vim
:reg
```

or:

```vim
:registers
```

to see the current contents of the registers.

To inspect specific registers:

```vim
:reg 0
:reg a
```

When something I wanted appears to have been overwritten, `:reg` is the first thing to check.

#### Important Registers

| Register | Purpose |
| --- | --- |
| `""` | Unnamed/default register; normally used by yank, delete, and change |
| `"0` | Most recent yank |
| `"1` | Most recent substantial delete/change |
| `"2`–`"9` | Older delete history |
| `"a`–`"z` | Named registers for explicitly storing text |
| `"_` | Black-hole register; deleted text is discarded |
| `"+` | System clipboard |

The most important distinction is between the unnamed register and register `0`.

If I yank something:

```vim
yiw
```

it becomes the most recent yank and is stored in `"0`.

If I then use:

```vim
ci[
```

the changed text affects the unnamed register, but my previous yank is still available in `"0`.

This means I generally do **not** need to use the black-hole register for every delete/change just to preserve something I deliberately yanked.

#### Registers in Normal Mode

The syntax is:

```text
"<register><command>
```

Common examples:

| Command | Purpose |
| --- | --- |
| `"0p` | Paste the most recent yank |
| `"ap` | Paste register `a` |
| `"ayy` | Yank the current line into register `a` |
| `"adw` | Delete a word into register `a` |
| `"_dd` | Delete a line into the black-hole register |

The `"` tells Vim that the next character identifies a register.

#### Registers in Visual Mode

Register prefixes work with visual selections too.

After selecting text:

```vim
"ay
```

yanks the selection into register `a`.

To paste the most recent yank over a selection:

```vim
"0p
```

Normal Visual-mode `p` replaces the selected text and puts the replaced text into the unnamed register.

Capital `P` is useful when repeatedly replacing selections because it replaces the selection without putting the replaced text into the unnamed register.

#### Registers in Insert Mode

Registers can be inserted **without leaving Insert mode**:

```text
Ctrl-r <register>
```

Examples:

| Command | Purpose |
| --- | --- |
| `Ctrl-r 0` | Insert the most recent yank |
| `Ctrl-r a` | Insert register `a` |
| `Ctrl-r +` | Insert the system clipboard |

This is especially useful after a change operation.

For example, if `#0b4f8a` was previously yanked and the cursor is inside:

```text
bg-[var(--color-primary)]
```

use:

```text
ci[ → Ctrl-r 0 → Esc
```

Result:

```text
bg-[#0b4f8a]
```

`ci[` changes the contents of the brackets and enters Insert mode. The removed text can replace the unnamed register, but the previous yank remains in `"0`, so `Ctrl-r 0` retrieves it immediately.

Mental model:

- **What registers contain:** `:reg`
- **Last thing deliberately yanked:** `"0`
- **Use a register in Normal/Visual mode:** `"<register>...`
- **Insert a register while already in Insert mode:** `Ctrl-r <register>`
- **Keep something around explicitly:** `"a`, `"b`, etc.
- **Actually throw deleted text away:** `"_`

---

### Telescope

- Prefer filename search over manually browsing a file tree.
- `<leader>fd` finds files.
- `<leader>fg` searches project text.
- Neo-tree is available, but it is not the preferred navigation model.

---

### Neo-tree File Operations

Neo-tree is not the preferred way to find files, but it is useful for manipulating the project structure.

Press:

```text
?
```

inside Neo-tree to show its available keybindings when one is forgotten.

#### Common Operations

| Key | Purpose |
| --- | --- |
| `a` | Create a file/directory |
| `d` | Delete |
| `r` | Rename |
| `c` | Copy |
| `m` | Move |
| `y` | Copy to Neo-tree clipboard |
| `x` | Cut to Neo-tree clipboard |
| `p` | Paste from Neo-tree clipboard |
| `?` | Show Neo-tree help/keybindings |

#### Creating Files and Directories

Put the cursor on the desired parent directory and press:

```text
a
```

Enter a filename to create a file:

```text
example.vue
```

Use a trailing `/` to create a directory:

```text
images/
```

#### Renaming

Put the cursor on the file or directory and press:

```text
r
```

Then enter the new name.

#### Moving Files and Directories

For interactive moves inside a visible project tree, prefer cut/paste over manually entering destination paths.

Given:

```text
equipment/
images/
```

To move `equipment/` into `images/`:

1. Put the cursor on `equipment/`.
2. Press `x`.
3. Navigate to `images/`.
4. Press `p`.

Mental model:

```text
x → navigate to destination → p
```

For copying instead:

```text
y → navigate to destination → p
```

#### Using `m`

`m` opens a prompt for a destination path.

Be careful about assuming that a relative path such as `./images` is interpreted relative to whatever directory visually appears to be the parent in the tree.

When both locations are visible, the simpler workflow is:

```text
x → destination → p
```

This avoids destination-path ambiguity.

#### Quick Reference

| Operation | Workflow |
| --- | --- |
| Create | `a` |
| Rename | `r` |
| Delete | `d` |
| Move | `x` → destination → `p` |
| Copy | `y` → destination → `p` |
| Help | `?` |

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

- [Fedora Desktop](Fedora%20Desktop.md)
- [Hyprland](Hyprland.md)
- [Tmux](Tmux.md)
- [Git](Git.md)
