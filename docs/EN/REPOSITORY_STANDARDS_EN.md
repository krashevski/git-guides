# 📘 Repository Management Guidelines (Standardization) — Reincarnation Backup Kit

This document defines guidelines for repository maintenance, branching, commits, testing, and releases.
The goal is to maintain high code quality, a stable main branch, and development transparency.

## 1. Branching

### 1.1 Main Branches
* main — a stable, secure branch.
Contains final, functional, fully tested code.
* develop (optional) — an intermediate branch for integrating features and preparing releases.
If the project is small, develop can be omitted.

### 1.2 Feature Branches
Each task is implemented in a separate branch:
```bash
feature/<short-name-of-the-task>
```

Examples:
feature/language-switch
feature/disk-check-improvements
feature/messages-refactoring

### 1.3 Bugfix Branches
Bug fixes:
```bash
bugfix/<short-description>
```

Examples:
bugfix/mount-detection
bugfix/menu-return-loop

### 1.4 Release Branches
- If develop is used, the following is created before the release:
```bash
release/<version>
```

- Otherwise, releases are built directly from main.

### 1.5 Backup Branches
Before breaking changes, a protective branch is created:
```bash
backup/<date or task-name>
```

Example:
```bash
backup/2025-12-10-pre-refactor
```

## 2. Commit Rules

### 2.1 Commit Message Format
Use a meaningful, short, and understandable format:
```bash
<type>: <short description>
```

Types:
feat — new feature
fix — fix
refactor — internal logic improvements
docs — documentation changes
test — tests
build — project build/structure
style — minor edits without changing logic
chore — other maintenance

Examples:
```bash
feat: add language switching to menu.sh
fix: correct USB mount detection in show-system-mounts.sh
docs: update git workflow section
refactor: split messages.sh into modules
```

### 2.2 Requirements for publishing a commit
1. The commit contains a completed change.
2. Large changes should be grouped into a single logical commit to keep history clean and readable.

## 3. Merging Rules

### 3.1 Merging with main
1. Allowed only after full testing (see section 5).
2. Merging is performed with the following command:
Merging can be performed using either:
a) merge — preserves full commit history
b) squash merge — combines all commits into one
* squash is convenient if there was rough work or many small "fix" or "WIP" commits within a feature.
* squash creates a single commit in main, which makes the history cleaner.
3. Pushing to main is prohibited; all changes must go through feature branches and testing.

### 3.2 How to updating main from a feature branch
1. Updating a feature branch with the latest changes from main
```bash
git checkout main
git pull origin main
git checkout feature/xxx
git merge main
```

2. If there are any conflicts, fix them.
3. Test the package locally.

Only after testing can you merge into main.

4. Removing old branches
To keep the repository clean, you can remove unnecessary branches:
```bash
git checkout main
git pull
git branch -d feature/xxx
git push origin --delete feature/xxx
```

## 4. The Release Process

### 4.1 Version Numbering
The SemVer scheme is used:
```bash
MAJOR.MINOR.PATCH
```

Examples:
1.0.0 — first stable release
1.1.0 — new feature
1.1.1 — bug fix

### 4.2 Creating a Release
1. The code in main must be fully tested.
2. A changelog is compiled.
3. Create a git tag:
```bash
git tag -a v1.2.0 -m "Release 1.2.0"
```

4. Push the tag:
```bash
git push origin v1.2.0
```

## 5. Testing Requirements Before Publishing

Any changes must be tested on the local machine before being pushed to main.

## 6. OpenSource Code Style

The code should be human-readable, contain clear variable names, a neat format, and include comments where necessary.

## 7. Summary

This standard ensures:
- Mainline stability,
- Professional project quality,
- Reduced risk of regressions,
- Scheduled releases.
