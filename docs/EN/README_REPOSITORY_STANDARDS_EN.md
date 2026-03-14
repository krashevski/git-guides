# 📘 Repository management guidelines (standardization)

[🇬🇧 English](README_REPOSITORY_STANDARDS_EN.md) | [🇷🇺 Русский](../RU/README_REPOSITORY_STANDARDS_RU.md)

This document defines guidelines for repository maintenance, branching, commits, testing, and releases.
The goal is to maintain high code quality, a stable main branch, and development transparency.

## 1. Branching

### 1.1 The main branch and other branches
* main — a stable, secure branch.
Contains final, functional, fully tested code.
* develop (optional) — an intermediate branch for integrating features and preparing releases.
If the project is small, develop can be omitted.

### 1.2 Feature branches
Each task is implemented in a separate branch:
```bash
feature/<short-name-of-the-task>
```

Examples:
- feature/language-switch
- feature/disk-check-improvements
- feature/messages-refactoring

### 1.3 Bugfix branches
Bug fixes:
```bash
bugfix/<short-description>
```

Examples:
bugfix/mount-detection
bugfix/menu-return-loop

### 1.4 Release branches
- If develop is used, the following is created before the release:
```bash
release/<version>
```

- Otherwise, releases are built directly from main.

### 1.5 Backup branches
Before breaking changes, a protective branch is created:
```bash
backup/<date or task-name>
```

Example:
```bash
backup/2025-12-10-pre-refactor
```

### See also
- Working with Git and GitHub branches [README_GIT_BRANCHES_EN.md](README_GIT_BRANCHES_EN.md)
- Cheat sheet: safe upgrades via кebase [README_GIT_REBASE_EN.md](README_GIT_REBASE_EN.md)

## 2. Commit rules

### 2.1 Commit message format
Use a format that is meaningful, concise, and clear:
```bash
<type>: <short description>
```

Types:
- feat — new feature
- fix — fix
- refactor — internal logic improvements
- docs — documentation changes
- test — tests
- build — project build/structure
- style — minor edits without changing logic
- chore — other maintenance

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

### See also
- Commit message convention [COMMIT_GUIDELINES.md](README_GIT_COMMIT_GUIDELINES_EN.md)

## 3. Merging rules

### 3.1 Merging with main
1. Allowed only after full testing (see section 5).
2. Merging is performed with command:
- merge — preserves full commit history
- squash merge — combines all commits into one

> **[I] Squash**
> - squash is convenient if there was rough work or many small "fix" or "WIP" commits within a feature.
> - squash creates a single commit in main, which makes the history cleaner.

3. Pushing to main is prohibited; all changes must go through feature branches and testing.

### 3.2 How to update the master branch from the development branch
1. Update the main branch  with the latest changes from the development branch:
```bash
git checkout main
git pull origin main
git merge feature/xxx
git push origin main
```

2. Eliminate conflicts.
3. Test the package locally.
4. Merge into main branch only after testing.
5. Delete unnecessary branches to keep your repository clean:
```bash
git checkout main
git pull
git branch -d feature/xxx
git push origin --delete feature/xxx
```

### See also
- Recovering files after a failed git merge [README_RECOVER_AFTER_MERGE_EN.md](README_RECOVER_AFTER_MERGE_EN.md)

## 4. Release process

### 4.1 Version numbering
The SemVer scheme is used:
```bash
MAJOR.MINOR.PATCH
```

Examples:
- 1.0.0 — first stable release
- 1.1.0 — new feature
- 1.1.1 — bug fix

### 4.2 Creating a release
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

### See also
- Stable Release [README_GIT_RELEASE_STABLE_EN.md](README_GIT_RELEASE_STABLE_EN.md)

## 5. Testing requirements before publishing

Changes must be tested on the local machine before being pushed to main.

### See also
- Testing before merging to main [README_GIT_TESTING_EN.md](README_GIT_TESTING_EN.md)

## 6. OpenSource code style

The code should be human-readable, contain clear variable names, a neat format, and include comments where necessary.

## 7. Summary

This standard ensures:
- Mainline stability.
- Professional project quality.
- Reduced risk of regressions.
- Scheduled releases.

## See also

- Working with Git and GitHub branches [README_GIT_BRANCHES_EN.md](README_GIT_BRANCHES_EN.md)
- Deleting from GitHub Branches [README_GIT_BRANCHES_DELETE_EN.md](README_GIT_BRANCHES_DELETE_EN.md)
- Cheat sheet: safe upgrades via кebase [README_GIT_REBASE_EN.md](README_GIT_REBASE_EN.md)
- Commit message convention [COMMIT_GUIDELINES.md](README_GIT_COMMIT_GUIDELINES_EN.md)
- Recovering files after a failed Git merge [README_RECOVER_AFTER_MERGE_EN.md](README_RECOVER_AFTER_MERGE_EN.md)
- Stable Release [README_GIT_RELEASE_STABLE_EN.md](README_GIT_RELEASE_STABLE_EN.md)
- Testing before merging to main [README_GIT_TESTING_EN.md](README_GIT_TESTING_EN.md)
- Working safely with GitHub [README_GITHUB_SAFE_EN.md](README_GITHUB_SAFE_EN.md)
- [Git-Brandmauer — GitHub](https://github.com/krashevski/git-brandmauer/blob/main/README.md)
