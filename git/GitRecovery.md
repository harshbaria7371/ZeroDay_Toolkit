# Git Disaster Recovery Guide

## 1. Recovering Lost Commits (The Magic of Reflog)
If you accidentally did a `git reset --hard` or deleted a branch, your commits are usually not gone. Git keeps a log of where your HEAD has been.

```bash
# View the history of all actions
git reflog

# You'll see output like:
# abc1234 HEAD@{0}: reset: moving to HEAD~1
# def5678 HEAD@{1}: commit: Added new feature

# To restore the state to 'def5678', just reset to that hash or HEAD index:
git reset --hard def5678
# or
git reset --hard HEAD@{1}
```

## 2. Undoing the Last Commit (Keeping Changes)
If you committed too early but want to keep your file changes in your working directory.

```bash
git reset --soft HEAD~1
```

## 3. Undoing the Last Commit (Discarding Changes)
If you completely messed up and want to throw away the last commit and all its changes. **Warning: This deletes uncommitted work.**

```bash
git reset --hard HEAD~1
```

## 4. Modifying the Last Commit Message
If you just made a typo in your last commit message.

```bash
git commit --amend -m "New correct message"
```

## 5. Adding Missed Files to the Last Commit
If you forgot to stage a file before committing.

```bash
git add forgotten_file.txt
git commit --amend --no-edit
```

## 6. Discarding Uncommitted Changes
To throw away all current changes in your working directory (that haven't been committed).

```bash
# Discard changes to tracked files
git restore .

# Discard untracked files (new files)
git clean -fd
```
