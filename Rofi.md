# Rofi

## Purpose

Rofi is the canonical application launcher and general selection UI for the Hyprland desktop.

The primary launcher shortcut is:

```text
Super + Space
```

Rofi is also used by the clipboard-history workflow.

---

## Configuration

```text
~/.config/rofi/config.rasi
```

---

## Theme

Rofi participates in **Moonlit Night**.

See [Moonlit Night](Themes/Moonlit%20Night.md).

The launcher is intentionally more visually substantial than Waybar or tmux.

Current design:

- large centered panel
- dark navy background
- moon-blue border
- lantern-gold prompt
- large application icons
- generous spacing
- strong moon-blue selected row

The goal is for the launcher to feel closer to a game menu / command palette than a stock Linux utility dialog.

---

## Application Launcher

Hyprland launches:

```bash
rofi -show drun -show-icons
```

This is the normal application-launch workflow.

The older Wofi launcher variable in the Hyprland configuration is not part of normal daily use and can be removed during future cleanup.

---

## Clipboard

Clipboard history uses `cliphist` and Rofi together.

Rofi therefore gives the clipboard picker the same visual language as the application launcher automatically.

---

## Related

- [Hyprland](Hyprland.md)
- [Moonlit Night](Themes/Moonlit%20Night.md)
