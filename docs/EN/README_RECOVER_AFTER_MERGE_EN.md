# 📘 Recovering files after a failed git merge

[🇬🇧 English](README_RECOVER_AFTER_MERGE_EN.md) | [🇷🇺 Русский](../RU/README_RECOVER_AFTER_MERGE_RU.md)

This document describes a safe and reproducible procedure for recovering files after a failed git merge, including in system or distribution recovery situations.

## 🎯 Goal

1. Recover lost or overwritten files.
2. Preserve correct Git history.
3. Avoid --force and history rewriting.
4. Explicitly commit the recovery.

## 🧠 Key Principle

> [!] Branches are considered synchronized only through commits, not through the working tree.
Commands like:
```bash
git checkout <commit> -- <file>
```

**do not change history**, only the state of the files. Changes become part of the branch **only after a git commit**.

## 🟢 Scenario 1: The merge hasn't been committed yet

Check:
```bash
git status
```

If Git reports:
```bash
You are in the middle of a merge
All conflicts fixed, but you are still merging
```

➡️ Safe rollback:
```bash
git merge --abort
```

✔ The working tree and branch are returned to their pre-merge state.

## 🟡 Scenario 2: The merge is already committed, but files are lost

### 1️⃣ Find the commit with the correct version of the files
```bash
git log --oneline --decorate
```

Example:
```bash
8731eed good state before merge
```

### 2️⃣ Restore specific files
```bash
git checkout 8731eed -- scripts/show-system-mounts.sh
```

Repeat for all necessary files.

### 3️⃣ Check the state
```bash
git status
git diff
```

Make sure the desired changes were restored.

### 4️⃣ Be sure to commit the restore
```bash
git commit -m "fix: restore scripts after failed merge"
```

✔ The restore is now part of the branch history.

## 🔁 Synchronizing branches after restore

If the restore was made in main, and a branch previously existed (for example, i18n):
```bash
git checkout i18n
git merge main
```

Checking equivalence:
```bash
git diff main..i18n
```

If diff is empty, the branches match in content.

## ❌ What NOT to do

* git reset --hard without understanding the consequences
* git push --force in main
* consider the restore complete without a commit

## 🧩 Typical cause of the problem

* restore the distribution / home directory
* a branch (e.g. i18n) was ahead of the restored state
* git merge completed without conflicts, but the logic was overwritten

## ✅ Recommendations for the future

* move localization (i18n) to separate files
* before merging:
```bash
git diff main..feature
```

* always check branches after a system restore

## 📌 Summary

* Files can be safely restored even after a merge.
* The truth for Git is commits, not the current state of files.
* A deliberate fix commit is the correct end of the restore.

> [I] 🛠 This document is intended for use in projects where reliability, reproducibility, and safety of Git history are important.

## See also

- Repository management guidelines [README_REPOSITORY_STANDARDS_EN.md](README_REPOSITORY_STANDARDS_EN.md)
