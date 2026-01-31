# Pull Request Process

[🇬🇧 English](GIT_GUIDES_PULLl_REQUEST_PROCESS_EN.md) | [🇷🇺 Русский](../RU/GIT_GUIDES_PULLl_REQUEST_PROCESS_RU.md)

This document describes the standard Pull Request (PR) workflow for repository. Following these steps helps keep the codebase stable, reviewable, and easy to maintain.

## 1. Before You Start

Before opening a Pull Request, make sure that:
- You have a **GitHub account** and access to the repository (fork or direct access).
- Your local repository is **up to date** with the `main` (or `master`) branch.
- Your changes are **scoped**: one PR = one logical change.
Recommended:
- Read the `README.md` and related docs.
- Check existing **Issues** and **Pull Requests** to avoid duplication.

## 2. Create an Archive (Snapshot)

Before starting work on a significant change or library update:
1. Ensure the `main` branch is clean and stable.
2. Create a **tag** or **release archive** on GitHub:

```bash
git checkout main
git pull origin main
git tag -a vX.Y.Z -m "Stable snapshot before changes"
git push origin vX.Y.Z
```
3. (Optional) Create a GitHub Release based on this tag.
This archive acts as a recovery point and reference baseline.

## 3. Create a Feature Branch

Never work directly on `main`.

```bash
git checkout -b feature/short-description
```
Branch naming conventions:
- `feature/...` – new functionality
- `fix/...` – bug fixes
- `docs/...` – documentation only
- `refactor/...` – internal code improvements

## 4. Make Changes

While working on your branch:
- Keep commits **small and meaningful**.
- Write **clear commit messages**:
```text
Add backup architecture v2
Fix permission check for root-only commands
Update i18n messages (JA)
```

- Run tests or manual checks relevant to your changes.

## 5. Update Documentation

If your changes affect behavior, configuration, or usage:
- Update or add documentation in `docs/` or `Git Guides/`.
- Mention new scripts, flags, or breaking changes explicitly.
PRs that change behavior **must include documentation updates**.

## 6. Push Your Branch

```bash
git push origin feature/short-description
```

## 7. Open a Pull Request

On GitHub:
1. Open a Pull Request from your branch to `main`.
2. Fill in the PR description using the following structure:

### PR Title
Short, descriptive summary of the change.

### Description
- What was changed
- Why it was changed
- Any important design decisions

### Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Refactor
- [ ] Documentation
- [ ] Other

### Testing
- How was this tested?
- On which OS / distribution?

### Checklist
- [ ] Code builds without errors
- [ ] Scripts run successfully
- [ ] Documentation updated
- [ ] No unrelated changes included

## 8. Review Process

During review:
- Be open to feedback.
- Respond to comments clearly and respectfully.
- Make requested changes in **new commits** (do not rewrite history unless asked).
If needed:
```bash
git commit --fixup <commit>
```

## 9. Merging

A PR can be merged when:
- All requested changes are addressed.
- If automated checks are configured (GitHub Actions / CI), all of them must pass before merging.
- At least one approval is received, if required by branch protection rules.

Preferred merge strategy:
- **Squash and merge** for small PRs (combine all PR commits into a single commit)
- **Merge commit** for larger, structured changes

## 10. After Merge

After the PR is merged:
1. Delete the feature branch:
```bash
git branch -d feature/short-description
git push origin --delete feature/short-description
```

2. Pull the updated `main` branch locally.
3. (Optional) Create a new tag or release if the change is significant.

## 11. Notes

- Large architectural changes should be discussed **before** implementation.
- Breaking changes must be clearly labeled and documented.
- When in doubt, open a draft PR early.

Happy hacking 🚀

## See also

- Mini-Course on working with Git and GitHub [README.md](../../README.md)