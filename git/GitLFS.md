# Git Large File Storage (LFS)

Git is designed for text files. Storing large binary files (videos, high-res images, PSDs, compiled binaries) in standard Git bloats the repository and slows down cloning. Git LFS solves this by replacing large files in your repo with tiny text pointers, storing the actual file contents on a remote server.

## 1. Installation

**Windows:**
Download the installer from [git-lfs.github.com](https://git-lfs.github.com/) or use Winget/Scoop.

**Mac:**
```bash
brew install git-lfs
```

**Linux (Ubuntu):**
```bash
sudo apt-get install git-lfs
```

## 2. Initialize LFS
Run this once per user account on your machine:
```bash
git lfs install
```

## 3. Tracking Files in a Repository
Inside your repository, tell LFS which file types to manage.

```bash
# Track all MP4 videos
git lfs track "*.mp4"

# Track a specific large file
git lfs track "assets/huge-database.sql"
```

*Note: This creates or updates a `.gitattributes` file.*

## 4. Commit and Push
You must commit the `.gitattributes` file along with your large files.

```bash
git add .gitattributes
git add "*.mp4"
git commit -m "Add large video assets via LFS"
git push origin main
```

## 5. Useful Commands

**List tracked LFS files:**
```bash
git lfs ls-files
```

**Check LFS status:**
```bash
git lfs status
```

**Migrate existing large files to LFS (Advanced):**
If you accidentally committed large files without LFS, they are still in your history. You can rewrite history to use LFS:
```bash
git lfs migrate import --include="*.mp4"
```
