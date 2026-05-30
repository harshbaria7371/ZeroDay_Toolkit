# Global .gitignore Setup

A global `.gitignore` file ensures you never accidentally commit OS-specific files (like Mac's `.DS_Store` or Windows' `Thumbs.db`) or IDE-specific files (like `.vscode/` or `.idea/`) to any repository on your machine.

## 1. Create the Global Ignore File
First, create a `.gitignore_global` file in your user's home directory.

**Windows (PowerShell):**
```powershell
New-Item -Path $env:USERPROFILE\.gitignore_global -ItemType File
```

**Mac/Linux:**
```bash
touch ~/.gitignore_global
```

## 2. Configure Git to Use It
Tell Git to use this file for all repositories.

```bash
git config --global core.excludesfile ~/.gitignore_global
```

## 3. Recommended Content
Open the file in any text editor and add the following common exclusions:

```text
# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# IDE / Editor folders
.vscode/
.idea/
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

# Node.js
npm-debug.log
yarn-error.log

# Python
__pycache__/
*.py[cod]

# Logs
*.log
```

## 4. Apply to Existing Repos (If needed)
If you've already committed files that should be ignored, adding them to `.gitignore` won't remove them. You need to untrack them:

```bash
git rm -r --cached .
git add .
git commit -m "chore: clean up ignored files"
```
