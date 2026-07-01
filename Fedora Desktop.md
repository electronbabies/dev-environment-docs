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

#### Terminal

- Ghostty
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

- [[Hyprland]]
- [[Moonlander]]
- [[Neovim]]
- [[Tmux]]
- [[Docker]]
- [[Nextcloud]]
- [[Browser]]
- [[Android]]

---

## Rebuild Checklist

- Install Fedora
- Update the system
- Install DNF applications
- Install Flatpaks
- Clone dotfiles
- Configure Hyprland
- Restore Moonlander layout
- Configure Neovim
- Configure Docker
- Configure Nextcloud
- Configure Browser
- Verify SSH keys
- Verify Git configuration

---

## Things Future Gary Will Forget

- Signal is installed as a Flatpak.
- Vimium is disabled on Gmail and YouTube.
- ChatGPT works well with Vimium by pressing `Esc` then `f`.