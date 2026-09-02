# Yazi

## Purpose

This page documents my Yazi file manager setup and the workflows I actually use.

The goal is not to document every Yazi feature. It is to record the important controls, configuration decisions, and anything Future Gary is likely to forget.

Yazi is my primary interactive terminal file manager.

For simple or precise filesystem operations, the shell is still often better. Yazi is mainly for fast visual navigation, previews, selection, and everyday file management.

---

## Installation

On Fedora, Yazi was installed through the COPR repository:

```bash
sudo dnf copr enable lihaohong/yazi
sudo dnf install yazi
```

Verify the installation:

```bash
yazi --version
```

Launch Yazi:

```bash
yazi
```

---

## Configuration Files

Yazi configuration lives under:

```text
~/.config/yazi/
```

Important files:

| File | Purpose |
| --- | --- |
| `~/.config/yazi/yazi.toml` | Main Yazi configuration |
| `~/.config/yazi/keymap.toml` | Custom keybindings |
| `~/.config/yazi/theme.toml` | Theme configuration |
| `~/.config/yazi/init.lua` | Lua initialization and plugin configuration |

Not every file needs to exist. Only create configuration files when there is an actual reason to override the defaults.

---

## File Manager Philosophy

- Keep Yazi close to upstream defaults.
- Learn the default workflow before customizing it.
- Let tmux handle multiple terminal views instead of forcing Yazi into a complex pane layout.
- Use Yazi for visual and interactive file management.
- Use the shell for complicated, precise, scripted, or high-risk operations.
- Prefer trash over permanent deletion.
- Add plugins or custom keybindings only when real friction appears.

---

## Layout

Yazi's default interface uses three columns:

```text
Parent Directory | Current Directory | Preview
```

The preview pane can display or inspect many file types, including:

- text files
- source code
- images
- videos
- archives such as `.tar.gz`
- directories
- file metadata

This preview-first workflow is one of the main reasons to use Yazi instead of a traditional GUI file manager.

---

## Navigation

Yazi uses familiar Vim-style movement.

| Key | Action |
| --- | --- |
| `j` | Move down |
| `k` | Move up |
| `h` | Go to parent directory |
| `l` | Enter directory |
| `Enter` | Open selected item |
| `o` | Open selected item |
| `O` | Choose how to open selected item |
| `Esc` | Cancel / clear selection / leave mode |

### Important

Use:

```text
l
```

to enter a directory.

`Enter` means **open**, not necessarily "navigate into."

Depending on the configured opener, pressing `Enter` on a directory may open that directory in Neovim.

---

## Selection

| Key | Action |
| --- | --- |
| `Space` | Toggle selection |
| `v` | Enter visual selection mode |
| `Ctrl+a` | Select all |
| `Ctrl+r` | Invert selection |
| `Esc` | Clear selection |

Example:

```text
Space
j
Space
j
Space
```

This selects multiple individual files.

Visual mode is useful for selecting a range:

```text
v
jjjj
```

---

## File Operations

| Key | Action |
| --- | --- |
| `y` | Yank / copy |
| `x` | Cut |
| `p` | Paste |
| `d` | Move to trash |
| `D` | Permanently delete |
| `r` | Rename |
| `a` | Create file or directory |

To create a directory with `a`, end the name with `/`.

Example:

```text
images/
```

### Delete vs Permanent Delete

Use:

```text
d
```

for normal deletion.

This moves the file to the system trash and is the safe default.

Use:

```text
D
```

only when permanent deletion is intentional.

Treat `D` like using `rm` from the shell.

---

## No Universal Undo

Yazi does not provide a general filesystem undo command for operations such as:

- move
- copy
- rename
- permanent delete

Because of this:

- prefer `d` instead of `D`
- use the shell for complicated or risky filesystem operations
- use Git as the safety net when working inside repositories
- be deliberate when moving or overwriting important files

---

## Search and Fast Navigation

Yazi integrates with common terminal navigation tools.

| Key | Action |
| --- | --- |
| `s` | Search filenames |
| `S` | Search file contents |
| `z` | Fuzzy navigation |
| `Z` | Zoxide navigation |

These make it unnecessary to manually walk through long directory trees most of the time.

---

## Tabs

Yazi supports tabs for keeping multiple directories available in one instance.

| Key | Action |
| --- | --- |
| `tt` | Create a new tab |
| `1` - `9` | Switch directly to a tab |
| `[` | Previous tab |
| `]` | Next tab |

Tabs are useful when two or more directories are part of the same file operation.

---

## Multiple Yazi Instances

For workflows that benefit from two independent full-screen file views, use separate tmux windows.

Example:

```text
tmux window 1 -> source browser
tmux window 2 -> destination directory
```

This is preferred over forcing Yazi into a permanent dual-pane setup.

tmux handles the multiple views.

Yazi handles the files.

---

## Shared Yank State

Yazi is configured so yank state is shared between multiple Yazi instances.

This allows:

```text
Yazi instance A -> select files -> y
Yazi instance B -> p
```

This is especially useful with multiple tmux windows.

Configuration:

```lua
-- ~/.config/yazi/init.lua

require("session"):setup {
  sync_yanked = true,
}
```

After changing this configuration, restart existing Yazi instances.

### Example Workflow

A useful pattern when collecting files from multiple locations into one directory:

```text
tmux window 1:
    browse source directories
    select files
    y

tmux window 2:
    remain parked in destination directory
    p
```

Repeat as needed.

This gives most of the practical benefit of a two-pane file manager while preserving full-width previews in both Yazi instances.

---

## Background Tasks

Yazi can perform file operations asynchronously.

Open the task manager with:

```text
w
```

This is useful for inspecting long-running copy, move, archive, or filesystem operations without blocking normal navigation.

---

## Opening Files

Yazi uses configured system/application openers.

For development files, Neovim may be the selected opener.

General mental model:

```text
l       navigate into directory
Enter   open selected item
```

Keeping these actions mentally separate avoids accidentally launching Neovim when the intention was only to navigate.

---

## Yazi + tmux

Yazi fits into the terminal environment as:

```text
Kitty
└── tmux
    ├── Neovim
    ├── OpenCode
    ├── Yazi
    └── shell
```

tmux owns workspace organization.

Yazi owns interactive filesystem navigation.

This avoids duplicating pane/window management inside individual applications.

---

## Yazi + Shell

Yazi is not intended to replace normal shell commands.

Use Yazi when:

- browsing unfamiliar directories
- visually locating files
- previewing files
- selecting multiple files
- reorganizing normal files and directories
- exploring archives
- searching interactively
- collecting files from multiple locations

Use the shell when:

- the operation is complicated
- exact behavior matters
- wildcards or patterns make the operation easier
- using `find`, `fd`, `rsync`, `cp`, or `mv` is clearer
- the operation should be reproducible
- the operation is risky enough that seeing the exact command first is useful

---

## Core Commands to Remember

The minimum useful Yazi vocabulary:

```text
h j k l     navigate

Space       select
v           visual selection

y           yank / copy
x           cut
p           paste

d           trash
D           permanent delete

r           rename
a           create

s           filename search
S           content search

z           fuzzy navigate
Z           zoxide navigate

tt          new tab
1-9         switch tabs

w           task manager
```

If these commands are comfortable, most everyday file management can already be done efficiently.

---

## Current Intentional Customization

The current Yazi setup intentionally stays minimal.

Configured:

- shared yank state across Yazi instances

Not currently necessary:

- large plugin collections
- heavily customized keymaps
- custom multi-pane layouts
- replacing tmux functionality inside Yazi

Theming can be customized separately without changing the core workflow.

---

## Future Notes

When considering new Yazi configuration:

1. Use the default behavior first.
2. Identify repeated friction.
3. Make the smallest configuration change that fixes it.
4. Document why the change exists.

The configuration files are the implementation.

This document records the reasoning and workflow.
