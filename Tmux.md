# Tmux Cheat Sheet

> **Leader Key:** `Ctrl+a`

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

### Detach from the current session

```
Ctrl+a d
```

### Kill a session

```bash
tmux kill-session -t session-name
```

---

# Windows

### Create a new window

```
Ctrl+a c
```

### Rename the current window

```
Ctrl+a ,
```

### Close the current window

Type:

```bash
exit
```

or press:

```
Ctrl+d
```

### Next window

```
Ctrl+a n
```

### Previous window

```
Ctrl+a p
```

### Jump directly to a window

```
Ctrl+a 0
Ctrl+a 1
Ctrl+a 2
...
Ctrl+a 9
```

### List windows

```
Ctrl+a w
```

---

# Panes

### Split vertically (left/right)

```
Ctrl+a %
```

### Split horizontally (top/bottom)

```
Ctrl+a "
```

### Move between panes

```
Ctrl+a ←
Ctrl+a →
Ctrl+a ↑
Ctrl+a ↓
```

### Cycle through panes

```
Ctrl+a o
```

### Resize a pane

Hold:

```
Ctrl+a
```

Then press one of:

```
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

```
Ctrl+d
```

---

# Copy Mode

Enter copy mode:

```
Ctrl+a [
```

Navigation:

- Arrow Keys
- Page Up / Page Down
- Vim keys (if configured)

Exit:

```
q
```

---

# Miscellaneous

### Reload tmux configuration

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

```
Ctrl+a w
```

---

# Typical Project Layout

Use one tmux session as the workspace for a project or related group of repositories.

For a multi-repository application such as LangLife, separate major components into clearly named windows. A practical layout is:

```text
Window 1: API
Window 2: UI
Window 3: Android
Window 4: Shell / logs / supporting work
```

The exact numbering is less important than giving windows useful names. Direct window switching with `Ctrl+a` plus a number is preferred over repeatedly cycling with `n` and `p`.

For a simpler project, one Neovim window plus supporting shell/server windows may be enough.

---

# Daily Workflow

1. Attach to the project session.
2. Start any required services.
3. Develop normally.
4. Detach (`Ctrl+a d`) instead of closing the terminal.
5. Reattach later and continue exactly where you left off.