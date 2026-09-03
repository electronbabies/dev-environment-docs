# Waybar

## Purpose

Waybar is the Hyprland status bar.

It provides workspace state, time, and lightweight system information without becoming a dashboard that competes with the desktop.

---

## Configuration Files

```text
~/.config/waybar/config
~/.config/waybar/style.css
```

The JSON/JSONC configuration owns module behavior and layout.

The stylesheet owns appearance.

---

## Current Layout

### Left

- Hyprland workspaces

### Center

- Clock

### Right

- PulseAudio
- Network
- CPU
- Memory
- Temperature
- Backlight
- Battery
- Tray

The current structure is intentionally simple and did not need redesigning when Moonlit Night was added.

---

## Theme

Waybar participates in **Moonlit Night**.

See [Moonlit Night](Themes/Moonlit%20Night.md).

The original Waybar stylesheet used a different bright background color for nearly every module. This made the bar visually noisy and disconnected from the wallpaper.

The current design uses one dark navy surface with semantic color instead.

### Color Roles

- normal information: cool gray / soft white
- active workspace: moon blue
- warning / temperature emphasis: lantern gold
- critical / disconnected state: crimson
- positive / charging state: muted green

Most modules remain visually neutral until their state requires attention.

---

## Design Rule

Waybar should recede into the desktop.

The wallpaper provides atmosphere.

Waybar provides information.

Do not give every module its own decorative color.

---

## Restart

During configuration:

```bash
pkill waybar
waybar &
```

A clean Hyprland restart/reboot will also recreate it through the normal startup configuration.

---

## Known GTK Warning

Waybar may print repeated GTK accelerator warnings related to tray items when started from a terminal.

If the bar and tray otherwise work normally, these warnings can be ignored.

Do not redesign the Waybar configuration solely to silence harmless terminal output.

---

## Related

- [Hyprland](Hyprland.md)
- [Moonlit Night](Themes/Moonlit%20Night.md)
