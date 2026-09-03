# Fedora Desktop

## Purpose

This page documents everything required to rebuild my Fedora development environment.

The focus is on the software, tools, and configuration that make this workstation feel like *my* workstation. Detailed configuration belongs in the individual pages for each application.

---

## Operating System

- Fedora Workstation
- Hyprland (primary desktop environment)

---

## Installation Sources

### DNF

#### Desktop

- Hyprland
- Waybar
- Dunst
- Hypridle
- Kanshi
- cliphist
- Rofi
- swaybg

#### Terminal

- Kitty
- Zsh
- tmux

#### Development

- Git
- Docker
- Neovim
- Java
- Android Platform Tools (adb)
- CMake

#### Utilities

- btop
- fastfetch
- fd
- ripgrep
- fzf
- bat
- eza
- tree
- wl-clipboard

---

### COPR

#### Yazi

```bash
sudo dnf copr enable lihaohong/yazi
sudo dnf install yazi
```

---

### Flatpak

- Brave
- Obsidian
- Signal Desktop
- Slack
- Spotify
- Steam

---

### Manual Installation

- (Applications installed manually)

---

## Configuration

The configuration for each application is documented in its own page.

- [Hyprland](Hyprland.md)
- [Moonlit Night](Themes/Moonlit%20Night.md)
- [Waybar](Waybar.md)
- [Kitty](Kitty.md)
- [Rofi](Rofi.md)
- [Dunst](Dunst.md)
- [Moonlander](Moonlander.md)
- [Neovim](Neovim.md)
- [Tmux](Tmux.md)
- [Yazi](Yazi.md)
- [Nextcloud](Nextcloud.md)
- [Browser](Browser.md)

---

## Rebuild Checklist

- Install Fedora
- Update the system
- Install DNF applications
- Install Flatpaks
- Clone dotfiles
- Configure Hyprland
- Restore Moonlit Night theme and wallpaper
- Restore Moonlander layout
- Configure Kitty, Waybar, Rofi, and Dunst
- Configure Neovim
- Configure Yazi
- Configure Docker
- Configure Nextcloud
- Configure Browser
- Verify SSH keys
- Verify Git configuration

---

## Things Future Gary Will Forget

- Kitty is the primary terminal.
- Yazi is installed through the `lihaohong/yazi` COPR.
- `swaybg` provides the Hyprland wallpaper.
- Rofi is the canonical application launcher (`Super + Space`).
- Signal is installed as a Flatpak.
- Vimium is disabled on Gmail and YouTube.
- ChatGPT works well with Vimium by pressing `Esc` then `f`.
