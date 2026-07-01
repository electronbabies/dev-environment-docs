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
git clone git@github.com:electronbabies/dotfiles.git
cd dotfiles
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