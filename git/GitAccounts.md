# Multiple GitHub Accounts Setup on One Machine (Work + Personal)

## Overview
This document explains how to configure and use multiple GitHub accounts (work and personal) on the same machine using SSH keys and Git configuration.

## 1. Clean Existing Git Configuration
Check current global Git configuration:
```bash
git config --global --list
```

Remove incorrect values if needed:
```bash
git config --global --unset user.name
git config --global --unset user.email
```

## 2. Clean Existing SSH Keys (Optional)
Check existing SSH keys:
```bash
ls ~/.ssh
```

Backup and remove old keys if necessary:
```bash
mkdir ~/ssh_backup
mv ~/.ssh/id_* ~/ssh_backup/
```

## 3. Create SSH Directory
```bash
mkdir -p ~/.ssh
ls -la ~/.ssh
```

## 4. Generate SSH Keys
**Work Account:**
```bash
ssh-keygen -t ed25519 -C "work_email@company.com"
```
Save as: `~/.ssh/id_ed25519_work`

**Personal Account:**
```bash
ssh-keygen -t ed25519 -C "personal_email@gmail.com"
```
Save as: `~/.ssh/id_ed25519_personal`

## 5. SSH Folder Structure
```text
~/.ssh
 ├── id_ed25519_work
 ├── id_ed25519_work.pub
 ├── id_ed25519_personal
 └── id_ed25519_personal.pub
```

## 6. Create SSH Config File
Create: `~/.ssh/config`

**Content:**
```text
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes

Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes
```

## 7. Fix Permissions
```bash
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_ed25519_work
chmod 600 ~/.ssh/id_ed25519_personal
```

## 8. Add Public Keys to GitHub
Copy keys:
```bash
cat ~/.ssh/id_ed25519_work.pub
cat ~/.ssh/id_ed25519_personal.pub
```

Add them to:
**GitHub → Settings → SSH and GPG Keys → New SSH Key**

## 9. Test SSH Authentication
```bash
ssh -T git@github-work
ssh -T git@github-personal
```

**Expected message:**
`Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.`

## 10. Clone Repositories
**Work repositories:**
```bash
git clone git@github-work:ORG_NAME/repository.git
```

**Personal repositories:**
```bash
git clone git@github-personal:USERNAME/repository.git
```

## 11. Update Existing Repository Remote
Check remote:
```bash
git remote -v
```

Change remote:

**Work:**
```bash
git remote set-url origin git@github-work:ORG_NAME/repository.git
```

**Personal:**
```bash
git remote set-url origin git@github-personal:USERNAME/repository.git
```

## 12. Configure Commit Identity
**Work repository:**
```bash
git config user.name "Your Name"
git config user.email "work_email@company.com"
```

**Personal repository:**
```bash
git config user.name "Your Name"
git config user.email "personal_email@gmail.com"
```

## 13. Workspace-Based Git Identity (Recommended)
Example folder structure:
```text
~/workspaces/
    work/
    personal/
```

Edit `~/.gitconfig`:
```ini
[user]
    name = Your Name
    email = personal_email@gmail.com

[includeIf "gitdir:~/workspaces/work/"]
    path = ~/.gitconfig-work
```

Create `~/.gitconfig-work`:
```ini
[user]
    name = Your Name
    email = work_email@company.com
```

## 14. Verification Commands
Check remote repository:
```bash
git remote -v
```

Check Git email:
```bash
git config user.email
```

Check config origin:
```bash
git config --show-origin user.email
```

## Final Setup Architecture
```text
SSH Keys
 ├─ id_ed25519_work
 └─ id_ed25519_personal

SSH Routing
 ├─ github-work
 └─ github-personal

Git Identity
 ├─ ~/workspaces/work → work email
 └─ ~/workspaces/personal → personal email
```

## Common Issues Fix for multi-account setups:

### Root Cause
Two key signals:
1. https:// URL is being used
2. Git is authenticating as wrong account

So. 
- Repo is using HTTPS instead of SSH
- Git Credential Manager is sending the wrong GitHub account credentials

## Fix (Recommended Approach - Switch to SSH)

### Step 1 - Check Current Remote
Inside the repo:
```bash
git remote -v
```
Output will show something like:
```
origin	https://github.com/ORG/repo.git (fetch)
origin	https://github.com/ORG/repo.git (push)
```

### Step 2 - Switch to SSH
```bash
git remote set-url origin git@github-personal:harshbaria7371/ParsePort.git
```

### Step 3 - Test and Verify

```bash
git remote -v
```
Output should now show:
```
origin git@github-personal:harshbaria7371/ParsePort.git
```

### Step 4 - Push to GitHub
```bash
git push
```
Now it will:
- Use the SSH key for this account
- Automatically use the correct GitHub account
- Not require any username/password

