# Kitty

## Purpose

Kitty is the primary terminal emulator.

It provides the terminal surface used by tmux, Neovim, Yazi, OpenCode, and normal shell work.

---

## Configuration

```text
~/.config/kitty/kitty.conf
```

The Kitty configuration directory is managed through the dotfiles repository.

---

## Theme

Kitty participates in **Moonlit Night**.

See [Moonlit Night](Themes/Moonlit%20Night.md).

The terminal uses:

- deep navy background
- soft cool-white foreground
- moon-blue / cyan cool colors
- violet secondary accents
- lantern-gold / orange warm colors
- muted green
- crimson errors

The ANSI palette is intentionally softer than bright stock terminal colors while keeping strong readability.

---

## Opacity

Kitty remains fully opaque.

The wallpaper is visually detailed, so terminal transparency would reduce readability without adding useful information.

```text
background_opacity 1.0
```

---

## Design Rule

Kitty is the visual foundation for terminal applications.

Terminal applications can add their own semantic color, but the base terminal should stay calm and readable.

---

## Related

- [Tmux](Tmux.md)
- [Yazi](Yazi.md)
- [Neovim](Neovim.md)
- [Moonlit Night](Themes/Moonlit%20Night.md)
