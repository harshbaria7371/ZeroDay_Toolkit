# Git Productivity Aliases

Typing full Git commands can be tedious. Git allows you to set up shortcuts (aliases) in your global configuration.

## 1. How to Setup
You can add these by running the commands below, or by directly editing your `~/.gitconfig` file.

## 2. The Best Alias: `git lg`
This creates a beautiful, colorized, graph-based view of your commit history. It's infinitely better than `git log`.

**Run this command:**
```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

**Usage:**
```bash
git lg
git lg -10  # Show last 10 commits
```

## 3. General Shortcuts
These save keystrokes for common commands.

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

**Usage:**
- `git st` instead of `git status`
- `git co main` instead of `git checkout main`

## 4. Undo and Unstage
```bash
# Unstage a file (moves it out of the commit index)
git config --global alias.unstage 'reset HEAD --'

# View last commit changes
git config --global alias.last 'log -1 HEAD'
```

## 5. Reviewing Config
To see all your current aliases:
```bash
git config --get-regexp alias
```
