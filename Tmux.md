# Tmux Cheat Sheet

> **Leader Key:** `Ctrl+a`

---

# Configuration

Configuration file:

```text
~/.tmux.conf
```

The current configuration intentionally stays small and focuses on:

- `Ctrl+a` prefix
- Vim-style pane navigation
- Vim-style splitting
- new windows inserted after the current window
- new windows inheriting the current pane's working directory
- fast config reload
- Moonlit Night status/pane styling

Current new-window binding:

```tmux
bind c new-window -a -c "#{pane_current_path}"
```

This changes the normal `Ctrl+a c` behavior so that a new window:

- is inserted immediately after the current window instead of being appended to the end
- starts in the current pane's working directory
- causes later windows to be renumbered automatically

This is useful for keeping related project windows grouped together.

Reload after editing:

```text
Ctrl+a r
```

or:

```bash
tmux source-file ~/.tmux.conf
```

---

# Theme

tmux participates in **Moonlit Night**, but the status bar is intentionally visually quiet.

tmux should behave like workspace metadata rather than another large desktop UI layer.

Current visual roles:

- dark/transparent-looking status background
- muted inactive windows
- moon-blue active window
- lantern-gold session emphasis
- crimson alerts
- moon-blue active pane border
- dark inactive pane borders

See [Moonlit Night](<Themes/Moonlit Night.md>).

---

# Sessions

### Create a new session

```bash
tmux new -s session-name
```

Example:

```bash
tmux new -s ocr-japanese
```

### List sessions

```bash
tmux ls
```

### Attach to a session

```bash
tmux attach -t session-name
```

Example:

```bash
tmux attach -t ocr-japanese
```

### Create a grouped session

Create a new session that shares the same windows and panes as an existing session:

```bash
tmux new-session -t existing-session -s new-session
```

Example:

```bash
tmux new-session -t sugacoded -s sugacoded-agent
```

The sessions share windows and panes, but each client can independently select which window it is viewing. This is useful for working in the same tmux workspace from multiple terminals without both terminals being forced onto the same window.

### Detach from the current session

```text
Ctrl+a d
```

### Kill a session

```bash
tmux kill-session -t session-name
```

---

# Windows

### Create a new window

```text
Ctrl+a c
```

With the current configuration, the new window:

- is inserted immediately after the current window
- starts in the current pane's working directory

For example, starting with:

```text
1: API
2: CastCue
3: Shell
```

Creating a new window while viewing window 1 produces:

```text
1: API
2: New Window
3: CastCue
4: Shell
```

This makes it easy to keep related windows grouped together.

### Insert a window manually after the current window

Open the tmux command prompt:

```text
Ctrl+a :
```

Then run:

```text
new-window -a
```

To also inherit the current pane's working directory:

```text
new-window -a -c "#{pane_current_path}"
```

### Create, insert, name, and set the directory in one command

Open the tmux command prompt:

```text
Ctrl+a :
```

Example:

```text
new-window -a -n api-captures -c ~/code/langlife/langlife-api-captures
```

This creates a window immediately after the current one named `api-captures` and starts it in the specified directory.

### Rename the current window

```text
Ctrl+a ,
```

### Close the current window

Type:

```bash
exit
```

or press:

```text
Ctrl+d
```

### Next window

```text
Ctrl+a n
```

### Previous window

```text
Ctrl+a p
```

### Jump directly to a window

```text
Ctrl+a 0
Ctrl+a 1
Ctrl+a 2
...
Ctrl+a 9
```

### List windows

```text
Ctrl+a w
```

---

# Panes

### Split vertically (left/right)

```text
Ctrl+a v
```

The new pane starts in the current pane's working directory.

### Split horizontally (top/bottom)

```text
Ctrl+a s
```

The new pane starts in the current pane's working directory.

### Move between panes

```text
Ctrl+a h
Ctrl+a j
Ctrl+a k
Ctrl+a l
```

Pane movement follows Vim directions.

### Cycle through panes

```text
Ctrl+a o
```

### Resize a pane

Hold:

```text
Ctrl+a
```

Then press one of:

```text
Ctrl+←
Ctrl+→
Ctrl+↑
Ctrl+↓
```

*(Depending on your configuration, you may instead need to enter resize mode or use `Ctrl+a : resize-pane` commands.)*

### Close the current pane

Type:

```bash
exit
```

or press:

```text
Ctrl+d
```

---

# Copy Mode

Enter copy mode:

```text
Ctrl+a [
```

Navigation:

- Arrow Keys
- Page Up / Page Down
- Vim keys (if configured)

Exit:

```text
q
```

---

# Miscellaneous

### Reload tmux configuration

Preferred shortcut:

```text
Ctrl+a r
```

Manual command:

```bash
tmux source-file ~/.tmux.conf
```

### Show all key bindings

```bash
tmux list-keys
```

### Display current sessions

```bash
tmux ls
```

### Display current windows

```text
Ctrl+a w
```

---

# Typical Project Layout

Use one tmux session as the workspace for a project or related group of repositories.

For a multi-repository application such as LangLife, separate major components into clearly named windows.

A traditional layout might be:

```text
Window 1: API
Window 2: UI
Window 3: Android
Window 4: Shell / logs / supporting work
```

For agent-heavy development, group related implementation lanes together:

```text
Window 1: LangLife API / coordinator
Window 2: LangLife API / captures
Window 3: LangLife API / gaps
Window 4: LangLife API / study
Window 5: LangLife API / workers
Window 6: CastCue / OpenCode
Window 7: API shell
Window 8: CastCue shell
```

Because `Ctrl+a c` inserts a new window immediately after the current one, additional windows can be added directly beside the project or task they belong to rather than always appearing at the end of the session.

The exact numbering is less important than giving windows useful names and keeping related work grouped together.

Direct window switching with `Ctrl+a` plus a number is preferred over repeatedly cycling with `n` and `p`.

For a simpler project, one Neovim window plus supporting shell/server windows may be enough.

---

# Agent / Worktree Workflow

For parallel AI development, use one tmux window per Git worktree / implementation lane.

Example LangLife layout:

```text
~/code/langlife/api
    main
    coordinator / integration lane

~/code/langlife/langlife-api-captures
    api-03/captures
    Capture/OCR lane

~/code/langlife/langlife-api-gaps
    api-03/gaps
    CandidateGap/PersistentGap lane

~/code/langlife/langlife-api-study
    api-03/study
    Sentence/Study/Attempt lane

~/code/langlife/langlife-api-workers
    api-03/workers
    Provider/worker lane
```

Each OpenCode session should run from the directory belonging to its lane.

This keeps:

- working-tree changes isolated
- branch changes isolated
- agent context focused
- merges easier to review
- bad implementations easier to discard independently

The main worktree should remain the integration/coordinator lane rather than being used for feature implementation.

---

# Git Worktrees

### List worktrees

```bash
git worktree list
```

Example:

```text
/home/electronbabies/code/langlife/api                    59eb7a2 [main]
/home/electronbabies/code/langlife/langlife-api-captures 59eb7a2 [api-03/captures]
/home/electronbabies/code/langlife/langlife-api-gaps     59eb7a2 [api-03/gaps]
/home/electronbabies/code/langlife/langlife-api-study    59eb7a2 [api-03/study]
/home/electronbabies/code/langlife/langlife-api-workers  59eb7a2 [api-03/workers]
```

### Add a worktree with a new branch

```bash
git worktree add ../langlife-api-captures -b api-03/captures
```

### Merge a completed lane into main

From the main worktree:

```bash
cd ~/code/langlife/api
git merge api-03/captures
```

Repeat for other completed lanes as appropriate.

### Remove a finished worktree

```bash
git worktree remove ../langlife-api-captures
```

A worktree is another checked-out working directory of the same Git repository.

Each worktree has its own:

- branch
- working tree
- staging area
- uncommitted changes

All worktrees share the same Git object database and repository history.

Changes in one worktree do not modify another worktree until commits are explicitly merged, rebased, or cherry-picked.

A useful mental model is:

```text
Same Git repository
Different desks
Different branches
Different working files
```

---

# Daily Workflow

1. Attach to the project tmux session.
2. Start any required services.
3. Move to the relevant project or worktree window.
4. Develop normally.
5. Add related windows beside the current one with `Ctrl+a c`.
6. Keep separate OpenCode agents in separate worktree windows.
7. Use the main worktree/window for review and integration.
8. Detach with `Ctrl+a d` instead of closing the terminal.
9. Reattach later and continue exactly where you left off.