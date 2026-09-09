
tfiles

This repository is managed with [chezmoi](https://www.chezmoi.io/). Do not create manual symlinks for files such as `.zshrc` or `.gitconfig`; chezmoi applies the tracked source state to the correct paths in your home directory.

## Bootstrap a new macOS machine

### 1. Install Xcode Command Line Tools

```bash
xcode-select --install
```

### 2. Install Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow Homebrew's post-install output to add `brew` to your shell environment if prompted.

### 3. Install chezmoi

```bash
brew install chezmoi
```

### 4. Configure GitHub access

For a private repository, configure an SSH key and verify GitHub authentication before initializing:

```bash
ssh -T git@github.com
```

### 5. Initialize and apply dotfiles

```bash
chezmoi init --apply git@github.com:YOUR_GITHUB_USER/YOUR_DOTFILES_REPO.git
```

To inspect the changes before writing files:

```bash
chezmoi init git@github.com:YOUR_GITHUB_USER/YOUR_DOTFILES_REPO.git
chezmoi diff
chezmoi apply
```

### 6. Install Homebrew packages

After chezmoi has applied the managed `Brewfile`:

```bash
brew bundle --file="$HOME/Brewfile"
```

## Daily workflow

Edit a managed file through chezmoi, preview the result, apply it locally, and commit the source-state change:

```bash
chezmoi edit ~/.zshrc
chezmoi diff ~/.zshrc
chezmoi apply ~/.zshrc

chezmoi cd
git add -A
git commit -m "Update zsh configuration"
git push
```

If you edited a managed file directly in your home directory, import that live change back into chezmoi first:

```bash
chezmoi re-add ~/.zshrc
chezmoi cd
git add -A
git commit -m "Update zsh configuration"
git push
```

## Sync updates from GitHub

On an existing machine:

```bash
chezmoi update
```

Or review before applying:

```bash
chezmoi git pull -- --autostash --rebase
chezmoi diff
chezmoi apply
```

## Repository layout

Chezmoi translates source-state paths into files in the home directory:

| Source repository path | Applied target path |
| --- | --- |
| `dot_zshrc` | `~/.zshrc` |
| `dot_gitconfig` | `~/.gitconfig` |
| `dot_config/` | `~/.config/` |
| `dot_dotfiles/` | `~/.dotfiles/` |
| `Brewfile` | `~/Brewfile` |

Verify what chezmoi manages at any time:

```bash
chezmoi managed
```

Preview a full apply without changing files:

```bash
chezmoi apply --dry-run --verbose
```
