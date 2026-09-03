# Dunst

## Purpose

Dunst provides desktop notifications under Hyprland.

The goal is for notifications to remain readable and obvious without visually overpowering the desktop.

---

## Configuration

```text
~/.config/dunst/dunstrc
```

Dunst is started automatically by Hyprland.

---

## Test Notifications

Normal notification:

```bash
notify-send "Moonlit Night" "This is a test notification."
```

Urgency tests:

```bash
notify-send -u low "Low" "Low urgency notification"
notify-send -u normal "Normal" "Normal urgency notification"
notify-send -u critical "Critical" "Critical urgency notification"
```

---

## Theme

Dunst participates in **Moonlit Night**.

See [Moonlit Night](Themes/Moonlit%20Night.md).

All urgency levels share the same dark navy background.

Urgency is communicated primarily through the notification border:

- low: muted gray-blue
- normal: moon blue
- critical: crimson

This replaced the previous traffic-light style where the entire notification background changed color.

---

## Design Rule

Notifications should stand out because they appeared, not because a large colored rectangle took over part of the screen.

Keep the body calm and use borders for state.

---

## Restart

```bash
pkill dunst
dunst &
```

---

## Related

- [Hyprland](Hyprland.md)
- [Moonlit Night](Themes/Moonlit%20Night.md)
