# GitHub Workflow Guide

Panduan penggunaan Git dan GitHub untuk kolaborasi tim.

---

## 🚀 Initial Setup

### 1. Initialize Repository

```bash
# Navigate ke project folder
cd D:\src\Tugas\GameJamInternal

# Initialize git
git init

# Add all files
git add .

# First commit
git commit -m "Initial commit: Project structure and documentation

- Added folder structure for assets
- Added GDD (Game Design Document)
- Added Asset Guide
- Added Construct 3 Development Guide
- Added .gitignore for Construct 3 project

Co-Authored-By: Claude Code <noreply@anthropic.com>"
```

### 2. Create GitHub Repository

**Option A: Via GitHub Web**
1. Buka https://github.com/new
2. Repository name: `unexpected-pair-game`
3. Description: "Vertical endless runner dengan dual-world mechanics untuk Game Jam Internal"
4. Public atau Private (pilih sesuai kebutuhan)
5. **JANGAN** centang "Add README" (sudah ada di local)
6. Create repository

**Option B: Via GitHub CLI** (jika sudah install `gh`)
```bash
gh repo create unexpected-pair-game --public --source=. --remote=origin --description "Vertical endless runner dengan dual-world mechanics"
```

### 3. Connect & Push

```bash
# Add remote origin (ganti USERNAME dengan username GitHub)
git remote add origin https://github.com/USERNAME/unexpected-pair-game.git

# Push ke GitHub
git branch -M main
git push -u origin main
```

---

## 👥 Team Collaboration Setup

### Clone Repository (untuk anggota tim)

```bash
# Clone repo ke local
git clone https://github.com/USERNAME/unexpected-pair-game.git

# Navigate ke folder
cd unexpected-pair-game
```

### Git Configuration (setiap anggota tim)

```bash
# Set nama dan email
git config user.name "Nama Kamu"
git config user.email "email@example.com"

# Verify
git config --list
```

---

## 🌿 Branching Strategy

### Branch Structure

```
main (production-ready)
├── dev (development branch)
    ├── feature/player-movement
    ├── feature/enemy-system
    ├── feature/ui
    ├── feature/audio
    └── bugfix/collision-issue
```

### Branch Naming Convention

```
feature/[feature-name]     → Fitur baru
bugfix/[bug-name]          → Bug fix
hotfix/[critical-fix]      → Critical bug di main
art/[asset-name]           → Asset creation
docs/[doc-name]            → Documentation updates
```

**Examples:**
- `feature/mode-switching`
- `feature/powerup-system`
- `bugfix/player-fall-through-floor`
- `art/player-sprites`
- `docs/update-gdd`

---

## 🔄 Daily Workflow

### 1. Start Working (setiap hari)

```bash
# Pull latest changes dari main
git checkout main
git pull origin main

# Create new branch untuk task kamu
git checkout -b feature/nama-fitur

# Atau checkout ke existing branch
git checkout feature/nama-fitur

# Pull latest dari branch tersebut (jika collaborate)
git pull origin feature/nama-fitur
```

### 2. During Work (commit regularly)

```bash
# Check status
git status

# Add files
git add [file-name]              # Specific file
git add assets/sprites/player/   # Specific folder
git add .                        # All changes (hati-hati!)

# Commit dengan message yang descriptive
git commit -m "Add player Mode 1 movement logic

- Implemented jump mechanic
- Added melee attack hitbox
- Fixed collision with ground"

# Push ke remote branch
git push origin feature/nama-fitur
```

### 3. End of Day / Ready to Merge

```bash
# Make sure all changes committed
git status

# Push final changes
git push origin feature/nama-fitur

# Create Pull Request (via GitHub web atau CLI)
gh pr create --title "Add player Mode 1 movement" --body "Implemented jump and melee attack"
```

---

## 📝 Commit Message Guidelines

### Format
```
[Type]: Short summary (max 50 chars)

- Detailed point 1
- Detailed point 2
- Detailed point 3

[Optional: Reference issue #123]
```

### Types
- **feat**: Fitur baru
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code formatting (tidak mengubah functionality)
- **refactor**: Code refactoring
- **test**: Adding tests
- **chore**: Maintenance tasks

### Examples

**Good commits:**
```
feat: Add Mode 2 floating controls

- Implemented hold-to-descend, release-to-ascend
- Added smooth transition between modes
- Disabled platform behavior in Mode 2

fix: Player no longer falls through platforms after mode switch

- Added 0.1s delay before enabling Platform behavior
- Ensured collision is properly set

art: Add player Mode 1 sprites

- Idle animation (4 frames)
- Run cycle (8 frames)
- Jump animation (3 frames)
```

**Bad commits:**
```
"update"
"fix stuff"
"asdasd"
"commit before going home"
```

---

## 🔀 Merge Workflow

### Pull Request Process

1. **Create PR** setelah feature complete
   - Via GitHub web: Pull Requests → New Pull Request
   - Via CLI: `gh pr create`

2. **Fill PR Template:**
   ```markdown
   ## Description
   Brief description of what this PR does
   
   ## Changes
   - Change 1
   - Change 2
   
   ## Testing
   - [ ] Tested in browser
   - [ ] No console errors
   - [ ] Works in both modes
   
   ## Screenshots (jika UI/visual changes)
   [Add screenshot]
   ```

3. **Code Review:**
   - Request review dari minimal 1 anggota tim
   - Address feedback/comments
   - Push additional commits jika ada revisi

4. **Merge:**
   - Setelah approved, merge via GitHub
   - Pilih "Squash and merge" untuk clean history
   - Delete branch setelah merge

### Resolve Merge Conflicts

```bash
# Update local main
git checkout main
git pull origin main

# Merge main ke feature branch
git checkout feature/nama-fitur
git merge main

# Jika ada conflict:
# 1. Open conflicted files
# 2. Resolve conflicts manually (pilih antara <<<< HEAD dan >>>> main)
# 3. Add resolved files
git add [resolved-file]

# Complete merge
git commit -m "Merge main into feature/nama-fitur"

# Push
git push origin feature/nama-fitur
```

---

## 🚨 Important Rules

### DO ✅
- **Commit often** dengan meaningful messages
- **Pull before push** untuk avoid conflicts
- **Work on separate branches** for each feature
- **Test before commit** untuk ensure tidak break game
- **Review PR** dari teammates
- **Communicate** di Discord/WhatsApp sebelum merge besar

### DON'T ❌
- **Jangan commit** file `.autosave.c3p` atau `.tmp.c3p`
- **Jangan push** langsung ke `main` tanpa PR
- **Jangan commit** asset source files (.psd, .ai) yang besar
- **Jangan force push** tanpa koordinasi tim: `git push --force`
- **Jangan merge** PR sendiri tanpa review (kecuali emergency)

---

## 📦 Construct 3 Specific Tips

### Saving .c3p File

**Best practice:**
1. Sebelum save, close semua event sheet/layout yang tidak perlu
2. Save as `.c3p` (single file project)
3. Commit `.c3p` file setelah testing

**Avoiding conflicts:**
- `.c3p` adalah binary file, sulit resolve conflict
- **Coordinate** dengan tim: satu orang edit logic, yang lain edit art
- Communicate di grup sebelum edit file utama
- Gunakan **Project Folders** format jika possible (easier to merge)

### Construct 3 Project Folders Format

**Jika tim decide pakai project folders:**

```bash
# In Construct 3: Menu → Project → Save as project folder
# This creates:
project-name/
├── project.c3proj        (project config)
├── eventSheets/          (individual event sheets as XML)
├── layouts/              (individual layouts as XML)
├── sounds/
└── images/

# Easier to merge karena per-file, bukan binary
```

**Git workflow sama, but merge conflicts easier:**
- Each event sheet = separate file
- Can see diff di GitHub
- Easier collaboration

---

## 🔍 Useful Git Commands

### Status & Info
```bash
git status                          # Check working tree
git log --oneline --graph           # View commit history
git branch                          # List branches
git remote -v                       # Show remote URLs
```

### Undo Changes
```bash
git checkout -- [file]              # Discard changes in file
git reset HEAD [file]               # Unstage file
git reset --soft HEAD~1             # Undo last commit (keep changes)
git reset --hard HEAD~1             # Undo last commit (DISCARD changes)
```

### Stash (temporarily save work)
```bash
git stash                           # Save current work
git stash pop                       # Restore saved work
git stash list                      # List all stashes
```

### Remote Management
```bash
git fetch origin                    # Fetch changes tanpa merge
git pull origin main                # Pull dan merge dari main
git push origin feature/branch      # Push branch ke remote
```

---

## 🎯 Milestones & Tags

### Create Tags untuk Releases

```bash
# Lightweight tag (simple)
git tag v0.1.0

# Annotated tag (recommended, with message)
git tag -a v0.1.0 -m "Pre-alpha: Core mechanics implemented"

# Push tag ke GitHub
git push origin v0.1.0

# Push all tags
git push origin --tags
```

### Tag Naming Convention

```
v0.1.0    → Initial playable prototype
v0.2.0    → All core mechanics done
v0.3.0    → All assets integrated
v0.4.0    → Polish & balancing
v1.0.0    → Game Jam submission ready
```

---

## 📊 GitHub Project Board (Optional)

### Setup Project Board untuk Task Management

1. Go to repository → Projects → New Project
2. Choose template: "Basic Kanban"
3. Create columns:
   - 📋 **Backlog** (tasks to do)
   - 🚧 **In Progress**
   - 👀 **Review** (PR created, waiting review)
   - ✅ **Done**

4. Add tasks as issues:
   - Go to Issues → New Issue
   - Title: "Implement player movement"
   - Assign to: Team member
   - Labels: `feature`, `high-priority`
   - Add to project board

5. Move cards sebagai progress

---

## 🆘 Troubleshooting

### Problem: Merge conflict di .c3p file

**Solution:**
```bash
# Option 1: Choose one version
git checkout --ours project.c3p      # Keep your version
# atau
git checkout --theirs project.c3p    # Keep their version

# Option 2: Communicate & re-do
# Coordinate: siapa yang re-implement changes manually
```

### Problem: Accidentally committed large file

**Solution:**
```bash
# Remove from staging (belum push)
git rm --cached [large-file]
git commit -m "Remove large file"

# Sudah push: gunakan BFG Repo Cleaner
# (See GitHub docs untuk detail)
```

### Problem: Wrong commit message

**Solution:**
```bash
# Belum push: amend last commit
git commit --amend -m "Correct message"

# Sudah push: accept it atau force push (coordinate team!)
git commit --amend -m "Correct message"
git push --force origin branch-name
```

---

## 📚 Resources

### Learn Git
- **Interactive Tutorial**: https://learngitbranching.js.org/
- **Git Cheat Sheet**: https://education.github.com/git-cheat-sheet-education.pdf
- **Visualizing Git**: https://git-school.github.io/visualizing-git/

### GitHub Docs
- **Pull Requests**: https://docs.github.com/en/pull-requests
- **Resolving Conflicts**: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts

---

## ✅ Quick Reference

### Start New Feature
```bash
git checkout main
git pull origin main
git checkout -b feature/my-feature
# ... work ...
git add .
git commit -m "feat: Description"
git push origin feature/my-feature
```

### Update Feature Branch with Latest Main
```bash
git checkout main
git pull origin main
git checkout feature/my-feature
git merge main
git push origin feature/my-feature
```

### Create & Merge PR
```bash
# Via GitHub CLI
gh pr create --title "Title" --body "Description"
gh pr list
gh pr merge [PR-number] --squash
```

---

**Last Updated**: 2026-09-12  
**Team**: [Tambahkan nama anggota tim]
