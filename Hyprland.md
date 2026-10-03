# Hyprland

## Purpose

This page documents my Hyprland desktop environment.

The goal is to recreate the desktop exactly as I expect it to behave after a fresh Fedora installation.

The Hyprland configuration files are the implementation. This page documents the design decisions, workflows, and visual integration of the desktop environment.

---

## Configuration Files

| File | Purpose |
| --- | --- |
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
- Hyprland owns window/session behavior; individual tools own their own workflows.
- Desktop theming should remain cohesive without sacrificing readability.

---

## Theme

Hyprland participates in the **Moonlit Night** desktop theme.

See:

- [Moonlit Night](<Themes/Moonlit Night.md>)

The Hyprland layer is intentionally subtle. The wallpaper provides most of the atmosphere while window chrome provides clear focus state without competing with application content.

### Window Chrome

Current visual behavior:

- Active border: moon-blue to violet gradient.
- Inactive border: dark blue-gray.
- Border size: 2 px.
- Window rounding: subtle.
- Shadows: dark navy / near-black.
- Normal windows remain fully opaque.

The active border is intentionally noticeable enough to identify focus while remaining restrained.

### Wallpaper

Wallpaper is handled by **swaybg** rather than Hyprland's built-in default wallpaper.

The Hyprland logo / mascot background is disabled.

Current wallpaper startup:

```text
swaybg -i <wallpaper-path> -m fill
```

The active wallpaper belongs to:

```text
themes/moonlit-night/wallpapers/
```

The current preferred wallpaper is the lantern-lit village / road scene.

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
- swaybg

The following special workspaces are also preloaded:

- Scratch Terminal
- Gmail
- Obsidian
- Spotify

---

## Daily Shortcuts

### Application Launcher

- `Super + Space`

Uses **Rofi** as the canonical application launcher.

### Terminal

- `Super + Q`

### File Manager

- `Super + E` — opens **Yazi** in Kitty

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

Screenshot actions are grouped together on the **Moonlander Nav layer** so the workflow does not depend on remembering modifier combinations.

### Select and Edit

Moonlander key:

- `SelectEdit`

Behavior:

- Select a region with the mouse.
- Open the capture in the screenshot editor.
- Edit or annotate before saving or copying.

This is the normal workflow when a screenshot needs cropping, annotation, or adjustment.

### Current Monitor to Clipboard

Moonlander key:

- `CurClip`

Behavior:

- Capture the currently focused monitor.
- Copy the image directly to the clipboard.
- Do not open the editor.
- Do not save a file.

Useful when sending a screenshot directly into ChatGPT, Signal, or another application.

### All Monitors to Clipboard

Moonlander key:

- `AllClip`

Behavior:

- Capture the complete three-monitor desktop.
- Copy the image directly to the clipboard.
- Do not open the editor.
- Do not save a file.

### Current Monitor to File

Moonlander key:

- `CurFile`

Behavior:

- Capture the currently focused monitor.
- Save directly to the Screenshots directory.
- Do not open the editor.

### All Monitors to File

Moonlander key:

- `AllFile`

Behavior:

- Capture the complete three-monitor desktop.
- Save directly to the Screenshots directory.
- Do not open the editor.

The screenshot controls are intentionally kept adjacent on the Moonlander so the available capture modes are visible from the keyboard layout rather than memorized as modifier combinations.

---

## Clipboard

Clipboard history is managed by **cliphist**.

Open clipboard history with:

- `Super + V`

The clipboard picker is displayed through Rofi and therefore inherits the Moonlit Night Rofi theme.

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
| --- | ---: |
| Brave | 3 |
| Steam Library | 6 |
| Steam Games | 4 |
| Slack | 5 |
| Signal | 5 |

---

## Desktop UI Integration

Hyprland starts and coordinates several parts of the desktop environment.

### Waybar

Waybar provides the top status bar and workspace display.

Its appearance is part of Moonlit Night, but Waybar owns its own module configuration and stylesheet.

### Dunst

Dunst provides desktop notifications.

Notifications use a shared dark background with urgency communicated through border color rather than large colored notification backgrounds.

### Rofi

Rofi is the primary launcher and also provides the clipboard picker.

`Super + Space` is the normal application-launch workflow.

The older Wofi launcher variable remains unnecessary for daily use and can be removed when cleaning the Hyprland configuration.

### swaybg

`swaybg` owns the desktop wallpaper.

Hyprland's built-in default wallpaper is disabled.

---

See: [TODO → Hyprland](TODO.md#hyprland)

---

## Related

- [Fedora Desktop](<Fedora Desktop.md>)
- [Moonlit Night](<Themes/Moonlit Night.md>)
- [Moonlander](Moonlander.md)
- [Neovim](Neovim.md)
- [Browser](Browser.md)
- [Tmux](Tmux.md)
