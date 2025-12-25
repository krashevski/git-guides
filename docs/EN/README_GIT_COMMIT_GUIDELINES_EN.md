# 📝 Commit Message Convention

[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

[🇬🇧 English](README_GIT_COMMIT_GUIDELINES_EN.md) | [🇷🇺 Русский](../RU/README_GIT_COMMIT_GUIDELINES_RU.md)

This project adheres to a simplified version of Conventional Commits.

## General Format
```bash
<type>: <short description>
```

Example:
```bash
docs: add README for safe GitHub usage
```

## 🔖 Commit Types Used

| Type | When to Use |
| ------------ | ------------------------------------------------- |
| `feat:` | Adding new functionality |
| `fix:` | Bug fix |
| `docs:` | Documentation changes (README, guides) |
| `refactor:` | Refactoring without changing behavior |
| `chore:` | Technical changes (scripts, CI, cleanup) |
| `test:` | Adding or changing tests |
| `build:` | Build or Dependency Changes |

## 📌 Examples of correct messages

docs: add README_GIT_SECURITY guidelines
fix: correct symlink creation logic
feat: add i18n support for setup-symlinks
refactor: simplify disk detection logic
chore: remove obsolete test files

### ⚠️ What to avoid
❌ Bad examples:
```text
Update files
Fix
Changes
README update
```

Why it's bad:
- unclear what exactly was changed
- difficult to read the history
- inability to automatically generate CHANGELOG

### 🧠 Why it's important
✔ clear commit history
✔ easy to read git log --oneline
✔ easier to find changes
✔ in the future, CHANGELOG.md can be automatically generated

### 🔍 Pre-commit check
Recommended before Commit:
```bash
git status
git diff
```

And make sure that:
- no `<<<<<<<`, `=======`, `>>>>>>>`
- the commit message reflects the essence of the change

## 🔧 Amend the last commit of the personal repository on the main branch

Amend a commit already pushed to GitHub:
```bash
git commit --amend -m "docs: README_GIT_GUIDES files removed"
```

Check:
```bash
git log --oneline -1
```

Should be:
```bash
xxxxxxx docs: README_GIT_GUIDES files removed
```

The hash will change—that's normal.

Gently update a commit on GitHub:
```bash
git push --force-with-lease
```

✔ safe
✔ won't overwrite other people's changes
✔ correct for a personal repository

## 🔧 Fix a commit without rewriting history (safe)

If the commit is already in main and you don't want to rewrite history:
```bash
git add .
git commit -m "fix: post-release fixes"
git push
```

History will remain clear and understandable.

Check the result:
```bash
git log --oneline -3
```
