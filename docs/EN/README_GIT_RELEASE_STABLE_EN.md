# Releasing a stable version

[🇬🇧 English](README_GIT_RELEASE_STABLE_EN.md) | [🇷🇺 Русский](../RU/README_GIT_RELEASE_STABLE_RU.md)

## 1️⃣ Preparing for release

1. Switch to the main development branch
Usually `main` or `master`:
```bash
git checkout main
git pull origin main
```

2. Check the status and ensure there are no uncommitted changes
```bash
git status
```

3. Ensure all changes are tested
* Run tests, linters, and build.

* Fix bugs.
A stable release should not break existing functionality.

## 2️⃣ Creating a release branch (optional)

If you want to prepare a stable branch separately:
```bash
git checkout -b release/stable
```

* This is useful if you're making bug fixes before releasing.
* Once the changes are prepared, the branch can be merged into `main`.

## 3️⃣ Updating the version

1. Change the version file (e.g. VERSION or package.json) to the desired version:
```text
1.2.3
```

2. Commit the changes:
```bash
git add VERSION
git commit -m "Bump version to 1.2.3 for stable release"
```

[I] If there is no VERSION script, then you can save the change history in the CHANGELOG.md script

## 4️⃣ Tagging the stable version

Git typically uses annotated tags for releases:
```bash
git tag -a stable -m "Stable release for git-guides"
```

> [i] If you prefer, you can also specify a specific version:
> [i] git tag -a v1.2.3 -m "Release v1.2.3 stable"

Check the tag:
```bash
git tag
```

## 5️⃣ Publishing a branch and tag to a remote repository

```bash
git push origin main # or release/stable
git push origin stable # push the stable tag
```

The `stable` tag is now available on GitHub/GitLab.

## 6️⃣ Checking on a remote repository

1. List all tags:
```bash
git fetch --tags
git tag -l
```

2. Check for changes associated with the `stable` tag:
```bash
git log stable
```

## 7️⃣ Maintenance and hotfixes

If bugs are discovered after a release:
1. Create a branch for the hotfix:
```bash
git checkout -b hotfix/fix-typo stable
```

2. Make the fix, commit, update the tag, or create a new one:
```bash
git commit -am "Fix typo in guides"
git tag -f stable # rewrite the stable tag with the fixed version
git push origin hotfix/fix-typo
git push origin stable --force
```

[!] ⚠️ Use --force Carefully, so as not to break other team members' local copies.

## Process summary

1. Preparation and Tests → main
2. (Optional) Create a release branch
3. Update the version
4. Create an annotated tag "stable"
5. Publish the branch and tag
6. Check in on the remote repository
7. Hotfixes and update the tag if necessary

## See also

- Repository Management Guidelines [REPOSITORY_STANDARDS_EN.md](README_REPOSITORY_STANDARDS_EN.md)