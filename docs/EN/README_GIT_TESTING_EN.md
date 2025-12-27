# 🧪 Testing before merging to main

[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

[🇬🇧 English](README_GIT_TESTING_EN.md) | [🇷🇺 Русский](../RU/README_GIT_TESTING_RU.md)

This document describes the standard testing procedure recommended before merging any branch into main.

This document is intended for repositories following GitHub best practices and is especially useful for projects with:
- shell scripts
- distributions
- backup/restore logic

## 🎯 Why is the testing phase necessary?

Before merging into main, you must ensure that:
- main remains a **stable branch**
- changes from feature/i18n don't break the project
- there are no Git artifacts (conflict markers, temporary files) in the repository

This is in line with the GitHub philosophy:

> [I] **main must always be deployable**

## 🔀 When to perform testing

Testing is required if:
- the branch has been isolated for a long time
- git merge or git rebase have been performed
- conflicts have occurred
- scripts, menus, i18n, or documentation have been edited

## 🧩 Step 1: Checking the status Git
```bash
git status
```

Expected:
- working tree clean
- active branch not main

Checking diffs:
```bash
git diff main..HEAD
```

Changes should be **intentional and expected**.

## 🔍 Step 2: Checking for Git conflicts
```bash
grep -R "<<<<" .
```

Expected:
- empty output

This eliminates accidentally committed conflict markers.

## 📁 Step 3: Checking the Project Structure

- All .sh files have #!/bin/bash
- Scripts are executable:
```bash
chmod +x scripts/*.sh
```

## 🌍 Step 4: Testing i18n (if applicable)

- The menu launches without errors
- Messages are correct
- No empty lines
```bash
./scripts/menu.sh
```

## 🧪 Step 5: Functional Testing

Minimum:
- Running main scripts
- Dry-run mode (if applicable)
```bash
./scripts/menu.sh
```

## 📄 Step 6: Checking the Documentation

- README.md is up-to-date
- Links are working
- No conflicting markers

## ✅ Step 7: Preparing for Merge

Before merge:
```bash
git checkout main
git pull origin main
```

Then:
```bash
git merge <branch>
```

or (for a single file):
```bash
git checkout <branch> -- README.md
```

## 🚀 Step 8: Final check of main

```bash
git status
git diff origin/main..main
```

If everything is clear:
```bash
git push origin main
```

## 🧠 Recommendation

- Document this process in the repository
- Add a link to this document in the README
- Use it as a checklist, not a script

> [I] 🛡️ A good merge is not one that passes, but one that doesn't break main.

## See also

- Working safely with GitHub [README_GITHUB_SAFE_EN.md](README_GITHUB_SAFE_EN.md)
- Repository Management Guidelines [README_REPOSITORY_STANDARDS_EN.md](README_REPOSITORY_STANDARDS_EN.md)
