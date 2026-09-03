# Dotfiles

My configuration files are stored in a separate Git repository from the documentation.

Repository structure:

- `~/.config/`
- `~/.gitconfig`
- `~/.bashrc`
- `~/.zshrc`
- `themes/`

## Installation

Clone the repository:

```bash
mkdir -p ~/code
git clone git@github.com:electronbabies/dotfiles.git ~/code/dotfiles
cd ~/code/dotfiles
```

Run the installer:

```bash
./install.sh
```

The installer:

- Backs up existing configuration files.
- Replaces them with symbolic links.
- Can be run multiple times safely.

## Why symlinks?

Configuration files are edited directly from the repository, so every change is automatically tracked by Git. There is no need to manually copy updated files back into the repository.

## Repository Location

Canonical location:

```text
~/code/dotfiles
```

The installer creates symbolic links from home/configuration paths into this repository. Some links use absolute paths, so moving the repository can break them.

After intentionally moving the repository, rerun:

```bash
cd ~/code/dotfiles
./install.sh
```

Then check for broken symlinks:

```bash
find ~ -maxdepth 3 -xtype l -print
```

This was tested when moving the repository from `~/Projects/dotfiles` to `~/code/dotfiles`; rerunning the installer repaired the managed links.


## Themes

Desktop themes are version controlled alongside the application configuration.

Current theme:

```text
~/code/dotfiles/themes/moonlit-night/
```

Theme assets include:

- wallpapers
- application-specific colors
- visual overrides for Hyprland, Waybar, Kitty, tmux, Yazi, Rofi, Neovim, and Dunst

The theme is a visual layer, not a replacement for application configuration.

Keep behavior with the application. Keep appearance with the theme whenever practical.

Applications may consume theme files through:

- symbolic links
- `source`
- `include`
- imports
- application-specific override files

The repository is intentionally structured to support multiple themes later:

```text
themes/
├── moonlit-night/
├── plain-black/
└── ...
```

See [Moonlit Night](Themes/Moonlit%20Night.md).

## Personal Commands

Environment scripts live under:

```text
~/code/dotfiles/bin
```

That directory is on `PATH`.

Current command: `repos`.

See [Projects](Projects.md).


## Repository Checkpoint Exceptions

The `repos` script recursively checks `~/code` and also supports explicit repositories that intentionally live elsewhere through its `EXTRA_REPOS` array.

Current exception:

```text
~/Nextcloud/ObsidianVault/Infrastructure
```

This keeps filesystem organization based on purpose rather than forcing every Git repository into the development-project tree.
