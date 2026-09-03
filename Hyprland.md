# Hyprland

## Purpose

This page documents my Hyprland desktop environment.

The goal is to recreate the desktop exactly as I expect it to behave after a fresh Fedora installation.

The Hyprland configuration files are the implementation. This page documents the design decisions, workflows, visual integration, and anything Future Gary is likely to forget.

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

Screenshot bindings currently work well enough for normal use. Do not redesign the screenshot workflow unless it becomes actual friction, especially because keyboard-layer interactions can complicate Print Screen combinations.

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

## Things Future Gary Will Forget

- `Alt + Tab` only cycles **floating** windows.
- Workspace switching is driven by the Moonlander keyboard (F13–F18), not `Super + 1..6`.
- Gmail, Spotify, Obsidian, and the scratch terminal are **special workspaces**, not normal workspaces.
- Clipboard history is available with `Super + V`.
- Screenshot behavior is implemented by a custom script.
- Applications are automatically assigned to workspaces; don't move them manually unless there's a reason.
- Kanshi reloads automatically after monitors wake from DPMS.
- The wallpaper is provided by `swaybg`, not Hyprland itself.
- Hyprland's default logo / mascot wallpaper is intentionally disabled.
- `Super + Space` launches Rofi; Wofi is not part of the normal launcher workflow.
- Moonlit Night window chrome is intentionally subtle. The wallpaper and application themes carry most of the visual identity.

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
