# Dotfiles

My configuration files are stored in a separate Git repository from the documentation.

Repository structure:

- ~/.config/
- ~/.gitconfig
- ~/.bashrc
- ~/.zshrc

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

## Personal Commands

Environment scripts live under:

```text
~/code/dotfiles/bin
```

That directory is on `PATH`.

Current command: `repos`.

See [[Projects]].
