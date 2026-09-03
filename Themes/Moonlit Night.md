# Moonlit Night

## Purpose

Moonlit Night is the primary visual theme for the desktop environment.

The goal is not to make every application identical. The goal is to make the entire environment feel intentional and cohesive while preserving readability and the strengths of each individual tool.

The theme is inspired by:

- moonlit Japanese villages and shrine paths
- dark fantasy adventure-game atmosphere
- deep navy night skies
- warm lantern light
- muted crimson foliage and architecture
- soft moon-blue highlights
- restrained violet accents

The overall rule is:

> Dark navy is the canvas. Color is information. The wallpaper provides atmosphere.

---

## Theme Location

Theme assets live in the dotfiles repository under:

```text
themes/moonlit-night/
```

This directory contains the wallpaper set and application-specific theme files.

The active theme may be linked or sourced into application configuration directories as needed.

Application behavior should remain separate from appearance whenever practical.

---

## Design Principles

- Keep the desktop predominantly dark and neutral.
- Use saturated colors sparingly.
- Use color to communicate state rather than decorate every module.
- Preserve strong contrast for active and selected elements.
- Avoid transparency when it reduces readability against detailed wallpapers.
- Allow functional applications such as editors and file managers to use more color when it improves scanning and semantic distinction.
- Prefer application-specific theme overrides over replacing entire upstream configurations.
- Keep configuration small and intentional.

---

## Core Palette

| Role | Color | Hex |
| --- | --- | --- |
| Background | Deep navy-black | `#070b14` |
| Dark background | Near black | `#050810` |
| Surface | Dark blue-gray | `#101522` |
| Surface alternate | Slate navy | `#202a3b` |
| Foreground | Soft cool white | `#dce3f2` |
| Muted foreground | Blue-gray | `#a9b4c8` |
| Dim foreground | Muted slate | `#6f7b91` |
| Primary | Moon blue | `#7aa2f7` |
| Bright primary | Icy blue | `#9ecbff` |
| Cyan | Cool moonlight | `#7dcfff` |
| Secondary | Violet | `#bb9af7` |
| Warm accent | Lantern gold | `#e0a85c` |
| Warm secondary | Burnt orange | `#d98752` |
| Success | Muted green | `#8fcf8f` |
| Error / urgent | Crimson | `#c94f5c` |

These colors are not required to map identically in every application.

The important part is preserving their semantic roles.

---

## Semantic Color Roles

### Moon Blue

Used for active workspaces, focused windows, active tabs, primary selection, and structural focus indicators.

Moon blue is the main active-state color for the environment.

### Lantern Gold

Used for warnings, current-line emphasis, highlighted navigation state, secondary focus, and important non-critical information.

Lantern gold provides warm contrast against the otherwise cool environment.

### Crimson

Used for critical notifications, errors, urgent workspaces, and destructive or cut states.

Crimson should remain relatively rare so that it retains visual importance.

### Violet

Used for secondary selection, syntax roles, alternate emphasis, and marked states.

Violet supports the primary blue rather than competing with it.

### Green

Used for success, charging, copied state, and semantic syntax roles where useful.

Green should remain muted rather than fluorescent.

---

## Wallpapers

Moonlit Night includes multiple coordinated wallpapers built around the same palette.

The current preferred wallpaper is the lantern-lit village / road scene.

Other wallpapers include:

- moonlit shrine paths
- torii gates in mist
- mountain villages
- pagoda landscapes
- cliffside temples
- lantern-lit Edo-style streets

The wallpapers intentionally share deep navy skies, moon-blue highlights, warm lantern light, crimson or muted red accents, and large dark areas.

Because the palette is consistent across the set, wallpapers can be changed without retheming the environment.

---

## Wallpaper Philosophy

The wallpaper should provide atmosphere without competing with application content.

For detailed wallpapers:

- keep application backgrounds mostly opaque
- keep Waybar dark and restrained
- avoid excessive glass effects
- use strong active-state borders where necessary
- let previews and media content be visually rich while application chrome stays calm

Desktop icons are intentionally not used, which makes more detailed wallpapers practical.

---

## Application Integration

Moonlit Night currently themes:

- Hyprland
- Waybar
- Kitty
- tmux
- Yazi
- Rofi
- Neovim
- Dunst

Each application retains its own workflow and configuration.

Theme-specific changes should focus on appearance only unless a small behavior change is directly required for the visual system.

---

## Hyprland

Hyprland uses Moonlit Night for active and inactive window borders, shadows, rounding, and wallpaper presentation.

Active borders use moon blue with a restrained violet gradient.

Inactive borders are dark and intentionally subtle.

Normal application windows remain opaque for readability.

Wallpaper is handled separately through `swaybg`.

---

## Waybar

Waybar is intentionally restrained.

Normal modules use neutral cool-gray text rather than individual background colors.

Semantic states use:

- moon blue for active state
- lantern gold for warnings
- crimson for critical state
- muted green for positive state

The bar uses a dark navy background so it blends into the wallpaper rather than competing with it.

---

## Kitty

Kitty provides the terminal foundation for the theme.

The terminal uses a deep navy background, soft cool-white foreground, moon-blue and cyan cool colors, lantern gold and orange warm colors, and muted green and crimson ANSI colors.

The terminal remains fully opaque for readability.

---

## tmux

tmux is intentionally minimal.

It should function primarily as workspace metadata rather than another decorative UI layer.

The status bar uses muted inactive windows, a moon-blue active window, lantern-gold session emphasis, and crimson alerts.

Pane borders follow the same active/inactive hierarchy.

---

## Yazi

Yazi is allowed more functional color variation than the rest of the desktop.

File managers benefit from immediate visual distinction between directories, text files, images, media, archives, selected items, and copied or cut items.

For this reason, Yazi prioritizes scanning and state clarity over strict palette uniformity.

Directories are intentionally lighter than the first theme attempt because too much blue made file types and selection states blend together.

---

## Rofi

Rofi is the canonical application launcher.

It is launched with:

```text
Super + Space
```

The launcher uses:

- a large centered panel
- dark navy background
- moon-blue border
- lantern-gold prompt
- strong blue selected row
- large application icons
- generous spacing

The same Rofi theme also applies naturally to the clipboard picker.

The intent is for Rofi to feel closer to a game menu or command palette than a default Linux utility popup.

---

## Neovim

Neovim uses Tokyo Night Moon as the underlying colorscheme engine.

Moonlit Night overrides its palette and selected highlight groups.

The theme intentionally allows more semantic color variation inside code than elsewhere in the desktop.

Typical roles include:

- violet for keywords
- cyan for functions and methods
- icy blue for types
- green for strings
- gold for numbers and booleans
- orange for constants and special values
- muted blue-gray for comments
- crimson for errors

Readability takes priority over strict theme purity.

---

## Dunst

Dunst notifications use a consistent dark background.

Urgency is communicated primarily through border color:

- low: muted gray-blue
- normal: moon blue
- critical: crimson

This avoids large red, blue, or gray notification backgrounds that visually overpower the desktop.

---

## Theme Architecture

The theme is treated as a shared visual system rather than a monolithic application configuration.

Conceptually:

```text
themes/
└── moonlit-night/
    ├── wallpapers/
    ├── hypr/
    ├── waybar/
    ├── kitty/
    ├── tmux/
    ├── yazi/
    ├── rofi/
    ├── nvim/
    └── dunst/
```

Not every application must use this exact physical structure.

The important rule is:

- theme files belong to the theme
- behavior belongs to the application configuration
- applications may symlink, source, include, or import theme files as appropriate

This keeps the environment reproducible without coupling unrelated behavior to a visual theme.

---

## Future Theme Support

The repository structure should allow additional themes later.

For example:

```text
themes/
├── moonlit-night/
├── plain-black/
├── tokyo-night/
└── ...
```

The current theme should not assume it will always be the only theme.

Any future theme-switching system should preserve the same separation between palette, wallpaper, application appearance, and application behavior.

---

## Current Status

Moonlit Night is the active desktop theme.

The first-pass integration is complete across the primary environment.

Further changes should come from actual usage rather than continued tweaking for its own sake.
