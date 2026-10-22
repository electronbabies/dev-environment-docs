# Tmux Cheat Sheet

> **Prefix Key:** `Ctrl+a`

---

## Purpose

Tmux provides persistent terminal workspaces for software development, Neovim, OpenCode, and other project-specific processes.

The organizational structure is:

**Project → Session → Window → Pane**

The goals are:

- One tmux session per project.
- Window `0` is always a dedicated interactive zsh shell.
- Each OpenCode agent or development workstream gets its own window.
- Existing sessions and running processes are preserved when switching projects.
- Session and window navigation should be fast and keyboard-driven.
- Configured workspaces should be automatically created and reused.
- Configuration and workspace definitions are maintained in dotfiles.

---

## Configuration

### Configuration File

```text
~/.tmux.conf
```

The current configuration includes:

- `Ctrl+a` prefix
- Vim-style pane navigation
- Standard tmux splitting shortcuts
- Session and window pickers
- New windows inserted immediately after the current window
- New windows inheriting the current pane's working directory
- Fast configuration reload
- Moonlit Night status and pane styling

### New Window Binding

```tmux
bind c new-window -a -c "#{pane_current_path}"
```

When pressing `Ctrl+a c`, tmux:

- Creates a window immediately after the current window.
- Starts it in the current pane's working directory.
- Renumbers subsequent windows when automatic renumbering is enabled.

**Important:** In managed sessions, manually inserting a window can change the numbering of existing windows.

For permanent workstreams, prefer updating the workspace configuration.

### Reload Configuration

Shortcut:

```text
Ctrl+a r
```

Or from a shell:

```bash
tmux source-file ~/.tmux.conf
```

Reloading does not terminate existing processes.

---

## Theme

Tmux participates in **Moonlit Night**, but the status bar is intentionally visually quiet.

Tmux should behave like workspace metadata rather than another large desktop UI layer.

Current visual roles:

- Dark/transparent-looking status background
- Muted inactive windows
- Moon-blue active window
- Lantern-gold session emphasis
- Crimson alerts
- Moon-blue active pane border
- Dark inactive pane borders

See the Moonlit Night documentation.

---

## Keyboard Shortcuts

### Session Navigation

| Shortcut | Action |
|---|---|
| `Ctrl+a s` | Interactive session picker |
| `Ctrl+a (` | Previous session |
| `Ctrl+a )` | Next session |
| `Ctrl+a d` | Detach from tmux |

### Window Navigation

| Shortcut | Action |
|---|---|
| `Ctrl+a w` | Interactive window picker |
| `Ctrl+a 0–9` | Jump directly to numbered window |
| `Ctrl+a n` | Next window |
| `Ctrl+a p` | Previous window |
| `Ctrl+a l` | Previously active window |
| `Ctrl+a c` | Create a new window |
| `Ctrl+a ,` | Rename current window |
| `Ctrl+a &` | Kill current window (confirmation) |

### Pane Management

| Shortcut | Action |
|---|---|
| `Ctrl+a %` | Split vertically (left/right) |
| `Ctrl+a "` | Split horizontally (top/bottom) |
| `Ctrl+a h` | Move to left pane |
| `Ctrl+a j` | Move to lower pane |
| `Ctrl+a k` | Move to upper pane |
| `Ctrl+a l` | Move to right pane |
| `Ctrl+a o` | Cycle through panes |
| `Ctrl+a x` | Kill current pane (confirmation) |

**Note:** `Ctrl+a l` may be configured for right-pane navigation instead of the tmux default of switching to the previously active window. Check `tmux list-keys` for the effective binding.

### Other Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+a [` | Enter copy mode |
| `Ctrl+a :` | Open tmux command prompt |
| `Ctrl+a r` | Reload tmux configuration |
| `Ctrl+a ?` | Show key bindings |

Press the prefix first, release it, then press the next key.

---

## Sessions

A session represents one project workspace.

Each project should have its own session rather than mixing unrelated projects into a single session.

### Create a Session

```bash
tmux new-session -s session-name
```

Example:

```bash
tmux new-session -s castcue
```

### List Sessions

```bash
tmux list-sessions
```

Short form:

```bash
tmux ls
```

### Interactive Session Picker

```text
Ctrl+a s
```

Opens an interactive list of sessions.

Use the arrow keys to navigate and `Enter` to select.

This is the preferred way to move between project workspaces.

The binding should be:

```tmux
bind-key s choose-tree -s
```

### Switch Sessions

From inside tmux:

```bash
tmux switch-client -t vim-tutor
```

From the tmux command prompt (`Ctrl+a :`):

```text
switch-client -t vim-tutor
```

### Attach to a Session

From outside tmux:

```bash
tmux attach-session -t vim-tutor
```

### Detach from a Session

```text
Ctrl+a d
```

Detaching leaves running processes intact.

### Kill a Session

```bash
tmux kill-session -t session-name
```

**Warning:** Killing a session can terminate its windows and processes. Windows shared with other sessions may remain alive.

### Create a Grouped Session

Grouped sessions share the same windows and panes but allow each attached client to independently select its active window.

```bash
tmux new-session -t existing-session -s new-session
```

Example:

```bash
tmux new-session -t sugacoded -s sugacoded-agent
```

Grouped sessions are useful when accessing the same workspace from multiple terminals.

They are not necessary for normal project organization.

---

## Windows

A window represents a development workstream, OpenCode agent, editor, server, or supporting terminal.

### Window Numbering Convention

For managed project sessions:

- `0`: Dedicated zsh shell
- `1`: First OpenCode agent or workstream
- `2`: Second agent or workstream
- `3–9`: Additional agents or tools

Window `0` should remain available for general project navigation and commands.

### Create a Window

```text
Ctrl+a c
```

With the current configuration, the new window:

- Appears immediately after the current window.
- Inherits the current pane's working directory.

### Interactive Window Picker

```text
Ctrl+a w
```

Shows an interactive window tree.

Use the arrow keys to select a window and `Enter` to switch.

Unlike direct numbered shortcuts, the picker can display windows across sessions.

### Jump to a Window

```text
Ctrl+a 0
Ctrl+a 1
Ctrl+a 2
...
Ctrl+a 9
```

Direct number shortcuts are the preferred method for frequently used windows.

### Next and Previous Windows

```text
Ctrl+a n
Ctrl+a p
```

### Rename a Window

```text
Ctrl+a ,
```

### Create a Named Window

Open the tmux command prompt:

```text
Ctrl+a :
```

Then enter:

```tmux
new-window -a -n api -c ~/code/langlife/api
```

This creates a window named `api`, inserts it after the current window, and sets its working directory.

### Close a Window

Exit the shell:

```bash
exit
```

Or:

```text
Ctrl+d
```

If the window's only pane exits, the window normally closes.

Alternatively:

```text
Ctrl+a &
```

This requests confirmation before killing the window.

---

## Panes

Panes provide multiple terminal views inside a single window.

They are useful for short-lived supporting terminals, log monitoring, and commands that should remain visible alongside an editor.

### Split Vertically

Creates left and right panes:

```text
Ctrl+a %
```

The `%` character is normally typed using `Shift+5`.

### Split Horizontally

Creates top and bottom panes:

```text
Ctrl+a "
```

The `"` character is normally typed using `Shift+'`.

### Navigate Between Panes

```text
Ctrl+a h
Ctrl+a j
Ctrl+a k
Ctrl+a l
```

Navigation follows Vim directions:

| Key | Direction |
|---|---|
| `h` | Left |
| `j` | Down |
| `k` | Up |
| `l` | Right |

### Cycle Through Panes

```text
Ctrl+a o
```

### Resize a Pane

Use the tmux command prompt:

```text
Ctrl+a :
```

Example commands:

```tmux
resize-pane -L 5
resize-pane -R 5
resize-pane -U 5
resize-pane -D 5
```

These resize the active pane by five cells in the specified direction.

Additional resize shortcuts can be configured if needed.

### Close a Pane

```bash
exit
```

Or:

```text
Ctrl+d
```

To forcibly close a pane:

```text
Ctrl+a x
```

---

## Workspace Manager

The custom workspace manager provides declarative, reusable tmux project layouts.

It creates missing sessions and windows without restarting existing ones.

### Files

```text
~/code/dotfiles/tmux/
├── workspaces
└── workspaces.conf
```

Launcher symlink:

```text
~/.local/bin/workspaces
    → ~/code/dotfiles/tmux/workspaces
```

The launcher resolves its symlink before locating the configuration file.

### Configuration

Edit:

```bash
nvim ~/code/dotfiles/tmux/workspaces.conf
```

The configuration defines:

- Session names
- Window numbers
- Window names
- Working directories
- Optional startup commands

Each permanent workstream should be defined here.

### Available Commands

| Command | Action |
|---|---|
| `workspaces start` | Create missing workspaces and switch or attach |
| `workspaces sync` | Create missing sessions/windows without switching |
| `workspaces open vim-tutor` | Switch or attach to Vim Tutor |
| `workspaces list` | Show running sessions and windows |

### Start All Configured Workspaces

```bash
workspaces start
```

### Synchronize Without Switching

```bash
workspaces sync
```

This creates configured sessions and missing windows while leaving the current tmux client in its existing session.

### Open a Project

```bash
workspaces open vim-tutor
```

If the session already exists, the launcher reuses it.

If it does not exist, the launcher creates the configured session and windows.

### List Workspaces

```bash
workspaces list
```

### Idempotent Behavior

The launcher is designed to be safe to run repeatedly.

It should:

- Create missing sessions.
- Create missing configured windows.
- Preserve existing sessions.
- Preserve existing windows.
- Leave running OpenCode processes untouched.
- Avoid launching duplicate OpenCode processes in existing windows.

**Important:** Existing windows are not automatically moved or renamed. Window numbers and names should be kept consistent with the configuration.

### Startup Commands

A configured window can automatically start a command such as:

```bash
opencode
```

Commands run when the corresponding window is initially created.

Existing running commands are not restarted simply because the workspace launcher runs again.

### Shell Window

Every project session reserves window `0` for an interactive zsh shell.

This provides a permanent command prompt for:

- Git operations
- File navigation
- Builds and tests
- Project-specific commands
- Running temporary utilities

### Reboot and Process Persistence

Tmux keeps processes alive when clients detach, provided the tmux server remains running.

It does not automatically preserve processes through a reboot.

The workspace manager can recreate windows and restart configured programs after a reboot.

However, restoring an individual OpenCode conversation depends on OpenCode's own session persistence and resume capabilities.

---

## Project Workspace Organization

Each project gets one tmux session.

Each major development workstream or OpenCode agent gets its own window.

### Vim Tutor Example

```text
vim-tutor
├── 0: shell          (zsh)
├── 1: product        (OpenCode)
├── 2: plugin         (OpenCode)
├── 3: curriculum     (OpenCode)
├── 4: backend        (OpenCode)
└── 5: dashboard      (OpenCode)
```

### Other Projects

A larger working environment might contain:

```text
tmux
├── vim-tutor
│   ├── 0: shell
│   ├── 1: product
│   ├── 2: plugin
│   ├── 3: curriculum
│   ├── 4: backend
│   └── 5: dashboard
│
├── castcue
│   ├── 0: shell
│   ├── 1: android
│   └── 2: marketing
│
├── langlife
│   ├── 0: shell
│   ├── 1: api
│   ├── 2: android
│   └── 3: marketing
│
└── cooking
    ├── 0: shell
    ├── 1: api
    └── 2: web
```

These are illustrative layouts rather than mandatory configurations.

### Navigation

Use:

```text
Ctrl+a s
```

To switch projects.

Use:

```text
Ctrl+a 0–9
```

To switch workstreams inside the selected project.

### Design Principles

- Avoid creating a separate session for every OpenCode agent.
- Avoid mixing unrelated projects into one session.
- Keep window `0` available for general shell use.
- Name windows by responsibility rather than arbitrary identifiers.
- Prefer stable window assignments for established workstreams.
- Use temporary panes for supporting commands.
- Avoid unnecessary window proliferation.

The goal is to keep a large number of development processes manageable without requiring increasingly complex keyboard shortcuts.

---

## OpenCode Workflow

Each OpenCode agent should run in a dedicated tmux window.

This makes it possible to:

- Keep multiple agents active simultaneously.
- Switch between agents without interrupting them.
- Organize agents by project.
- Preserve running sessions while detaching.
- Return to an agent without navigating through unrelated terminals.

### Example

```text
vim-tutor
├── 0: shell
├── 1: architecture
├── 2: plugin
├── 3: curriculum
├── 4: backend
└── 5: dashboard
```

Each window should have an explicit responsibility.

### Working Directories

An OpenCode window should start in the appropriate repository or worktree directory.

Examples:

```text
~/code/vim-tutor
~/code/vim-tutor/plugin
~/code/vim-tutor/api
~/code/vim-tutor/web
```

Actual directory structure depends on the project architecture.

Directories configured before a repository exists may initially point to a common parent directory.

Update the configuration after the repository structure is established.

### Agents Do Not Always Need Worktrees

Separate OpenCode windows and separate Git worktrees solve different problems.

An OpenCode window isolates the terminal process and conversation.

A Git worktree isolates the checked-out branch and working files.

Research, planning, architecture, documentation, and curriculum-design agents may not require dedicated worktrees.

Parallel implementation agents editing overlapping areas of the same repository often benefit from worktree isolation.

Do not create worktrees unnecessarily.

---

## Git Worktrees

Git worktrees allow multiple branches of one repository to be checked out simultaneously.

They are useful when independent agents need to modify the same codebase without interfering with each other's working files.

### List Worktrees

```bash
git worktree list
```

### Create a Worktree

From the primary repository:

```bash
git worktree add ../feature-captures -b feature/captures
```

This creates a new directory with a new branch.

### Example Layout

```text
~/code/langlife/
├── api/
├── langlife-api-captures/
├── langlife-api-gaps/
├── langlife-api-study/
└── langlife-api-workers/
```

These represent possible independent implementation lanes.

They do not need separate tmux sessions.

All can belong to the same `langlife` tmux session, using different windows.

### Merge a Completed Branch

From the primary worktree:

```bash
cd ~/code/langlife/api
git merge feature/captures
```

Resolve conflicts, run verification, and confirm the result before removing the feature worktree.

### Remove a Worktree

```bash
git worktree remove ../feature-captures
```

Git may refuse removal if the worktree contains uncommitted changes.

### Worktree Isolation

Each worktree has its own:

- Checked-out branch
- Working files
- Staging area
- Uncommitted changes

Worktrees share the same repository object database and commit history.

Changes in one worktree do not directly alter another worktree's checked-out files.

Committed changes must still be integrated through Git operations such as merging, rebasing, or cherry-picking.

A useful mental model:

```text
Same repository
Different desks
Different branches
Different working files
```

---

## Copy Mode

### Enter Copy Mode

```text
Ctrl+a [
```

### Navigation

Depending on tmux configuration:

- Arrow keys
- Page Up / Page Down
- Vim navigation keys

### Exit Copy Mode

```text
q
```

With Vim copy-mode bindings enabled, `q` exits copy mode.

---

## Useful Commands

### Show All Key Bindings

```bash
tmux list-keys
```

### Check a Specific Binding

For example, to inspect the prefix-table binding for `s`:

```bash
tmux list-keys -T prefix s
```

### List Sessions

```bash
tmux list-sessions
```

### List Windows in the Current Session

```bash
tmux list-windows
```

### List Panes

```bash
tmux list-panes
```

### Show the Current Session Name

```bash
tmux display-message -p '#S'
```

### Show the Current Window Number and Name

```bash
tmux display-message -p '#I:#W'
```

### Show the Current Pane Directory

```bash
tmux display-message -p '#{pane_current_path}'
```

### Reload Configuration

```bash
tmux source-file ~/.tmux.conf
```

---

## Troubleshooting

### Session Picker Splits the Window Instead

A custom binding may be overriding the standard `s` key.

Inspect the effective binding:

```bash
tmux list-keys -T prefix s
```

Restore the session picker:

```bash
tmux bind-key s choose-tree -s
```

For a permanent fix, update the tracked tmux configuration and reload it.

### Window Picker Is Missing

Restore the default window tree:

```bash
tmux bind-key w choose-tree -w
```

### Workspace Configuration Cannot Be Found

The workspace launcher must resolve its actual path when invoked through a symlink.

The script uses:

```bash
SCRIPT_DIR="$(dirname -- "$(readlink -f -- "${BASH_SOURCE[0]}")")"
CONFIG="${WORKSPACES_CONFIG:-$SCRIPT_DIR/workspaces.conf}"
```

This ensures the configuration is loaded from the dotfiles directory instead of `~/.local/bin`.

### OpenCode Is Not Running in a Window

Inspect the window and its configured working directory.

If the window already existed when the workspace manager ran, it will not automatically relaunch OpenCode.

This behavior prevents existing processes from being interrupted.

Start OpenCode manually if needed:

```bash
opencode
```

### Previous Terminal Appears to Have Disappeared

Switching tmux sessions changes which workspace the client displays.

The previous session normally continues running.

Open the session picker:

```text
Ctrl+a s
```

Or list sessions:

```bash
tmux list-sessions
```

Then switch back:

```bash
tmux switch-client -t session-name
```

### Too Many Windows

Prefer moving unrelated workstreams into their corresponding project sessions.

Do not create additional sessions merely because one session becomes crowded.

Review whether inactive or completed agents still need dedicated windows.

For normal navigation, aim to keep frequently used windows within the `0–9` range.

---

## Daily Workflow

1. Open Kitty and attach to tmux.
2. Use `workspaces sync` if workspace configuration has changed.
3. Select the desired project with `Ctrl+a s`.
4. Switch to the relevant OpenCode agent using `Ctrl+a` plus its window number.
5. Use window `0` for general shell commands.
6. Use panes for temporary supporting terminals or log monitoring.
7. Keep independent implementation agents isolated through worktrees when necessary.
8. Review, test, and integrate completed implementation work.
9. Detach using `Ctrl+a d` instead of terminating tmux.
10. Reattach later and continue working with existing processes.

---

## Quick Reference

| Shortcut / Command | Action |
|---|---|
| `Ctrl+a s` | Select project/session |
| `Ctrl+a w` | Select window |
| `Ctrl+a 0–9` | Jump to window |
| `Ctrl+a n` | Next window |
| `Ctrl+a p` | Previous window |
| `Ctrl+a c` | New window |
| `Ctrl+a ,` | Rename window |
| `Ctrl+a %` | Split left/right |
| `Ctrl+a "` | Split top/bottom |
| `Ctrl+a h/j/k/l` | Navigate panes |
| `Ctrl+a d` | Detach |
| `Ctrl+a [` | Copy mode |
| `Ctrl+a r` | Reload configuration |
| `workspaces start` | Create/reuse configured workspaces |
| `workspaces sync` | Synchronize without switching |
| `workspaces open vim-tutor` | Open Vim Tutor workspace |
| `workspaces list` | Show workspaces |

---

## Core Principle

**One project, one session. One workstream, one window. One permanent shell at window 0.**

Keep the workspace predictable, preserve running processes, and use tmux to organize development rather than adding unnecessary complexity.