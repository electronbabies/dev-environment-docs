# Hyprland

## Purpose

This page documents my Hyprland desktop environment.

The goal is to recreate the desktop exactly as I expect it to behave after a fresh Fedora installation.

The Hyprland configuration files are the implementation. This page documents the design decisions, workflows, and anything Future Gary is likely to forget.

---

## Configuration Files

| File | Purpose |
|------|---------|
| `~/.config/hypr/hyprland.conf` | Main Hyprland configuration |
| `~/.config/hypr/hypridle.conf` | Lock screen, monitor power management, suspend behavior |
| `~/.config/hypr/startup.sh` | Alternative startup script (currently unused) |

---

## Desktop Philosophy

- Keyboard-first workflow.
- Three dedicated monitors.
- Predictable workspace layout.
- Frequently used applications are always one shortcut away.
- Temporary applications use **special workspaces** instead of occupying permanent workspaces.
- Mouse usage should be optional.

---

## Monitor Layout

### Left Monitor (HDMI)

- Workspace 1
- Workspace 2

### Center Monitor (Primary)

- Workspace 3
- Workspace 4

### Right Monitor

- Workspace 5
- Workspace 6

Applications are automatically assigned to specific workspaces whenever possible.

---

## Startup Applications

The following applications start automatically:

- Waybar
- Dunst
- Hypridle
- Kanshi
- Fcitx5
- Nextcloud
- Brave Browser
- Slack
- Signal

The following special workspaces are also preloaded:

- Scratch Terminal
- Gmail
- Obsidian
- Spotify

---

## Daily Shortcuts

### Application Launcher

- `Super + Space`

### Terminal

- `Super + Q`

### File Manager

- `Super + E`

### Close Window

- `Super + C`

### Fullscreen

- `Super + F`

### Clipboard History

- `Super + V`

### Floating Window

- `Super + V`

---

## Window Management

### Move Focus

- `Super + H`
- `Super + J`
- `Super + K`
- `Super + L`

### Move Windows

- `Super + Shift + H`
- `Super + Shift + J`
- `Super + Shift + K`
- `Super + Shift + L`

### Resize Windows

- `Super + Ctrl + H`
- `Super + Ctrl + J`
- `Super + Ctrl + K`
- `Super + Ctrl + L`

---

## Special Workspaces

### Scratch Terminal

Purpose:

A floating tmux terminal that is always available regardless of the current workspace.

Shortcut:

- `Super + \``

---

### Obsidian

Purpose:

Quick notes without leaving the current workspace.

Shortcut:

- `Super + O`

---

### Gmail

Purpose:

Dedicated Gmail application window.

Shortcut:

- `Super + G`

---

### Spotify

Purpose:

Music player available on demand without consuming a normal workspace.

Shortcut:

- `Super + S`

---

## Screenshots

### Area Selection

- `Print`

Uses the custom screenshot script.

### Full Screen

- `Shift + Print`

Saves directly to the Screenshots directory.

### Area to Clipboard

- `Ctrl + Print`

Copies directly to the clipboard.

---

## Clipboard

Clipboard history is managed by **cliphist**.

Open clipboard history with:

- `Super + V`

---

## Idle Behavior

### After 10 Minutes

- Lock session.

### After 15 Minutes

- Turn monitors off.
- Reload monitor configuration after wake.

### After 30 Minutes

- Suspend the computer.

---

## Automatic Workspace Assignment

The following applications automatically open on predefined workspaces:

| Application | Workspace |
|------------|-----------|
| Brave | 3 |
| Steam Library | 6 |
| Steam Games | 4 |
| Slack | 5 |
| Signal | 5 |

---

## Things Future Gary Will Forget

- `Alt + Tab` only cycles **floating** windows.
- Workspace switching is driven by the Moonlander keyboard (F13–F18), not `Super + 1..6`.
- Gmail, Spotify, Obsidian, and the scratch terminal are **special workspaces**, not normal workspaces.
- Clipboard history is available with `Super + V`.
- Screenshot behavior is implemented by a custom script.
- Applications are automatically assigned to workspaces; don't move them manually unless there's a reason.
- Kanshi reloads automatically after monitors wake from DPMS.

---

See: [TODO → Hyprland](TODO.md#hyprland)

---

## Related

- [[Fedora Desktop]]
- [[Moonlander]]
- [[Neovim]]
- [[Browser]]
- [[tmux]]