# 🔒 Working safely with GitHub

[🇬🇧 English](README_GITHUB_SAFE_EN.md) | [🇷🇺 Русский](../RU/README_GITHUB_SAFE_RU.md)

Goal: To provide a safe method for working with local repositories and remote services, minimizing the risk of token leaks, accidental force pushes, and other errors.

## 1. Setting up a local repository

### Clone the repository
```bash
git clone https://github.com/username/repository.git
cd repository
```

### Check the default branch
```bash
git branch
git status
```

Recommendations:
- Never work with the root account.
- Always checkout the current branch before committing.
- Create feature branches for new changes:

```bash
git checkout -b feature/name
```

## 2. Safely storing your Git token

### Using an environment variable (current session only):
```bash
export GIT_TOKEN="your_token_here"
```
Recommendations:
- Never store your Git token in a repository (.env, .bashrc, README, etc.).
- When finished, delete the variable:
```bash
unset GIT_TOKEN
```

### Never save the Git token in the repository:
```bash
# ❌ DO NOT
echo "GIT_TOKEN=..." >> .env
```

### Using a .git-credentials file with restricted access rights:
```bash
git config --global credential.helper store
chmod 600 ~/.git-credentials
```

> [i] In this case, the Git token will be stored locally, protected by user permissions.

## 3. Renewing a Git token with deletion

### Create a new token in GitHub
1. Open settings:
```bash
https://github.com/settings/tokens
```

2. Go to:
Settings → Developer settings → Personal access tokens.

3. Click:
Generate new token (or Regenerate token for an existing one).

4. Specify:
- Expiration period
- Required permissions (e.g., repo, workflow, etc.)
- Click Generate token.
- Copy the token — GitHub will only show it once.

### Method 1 — Delete the credential for GitHub (correctly)
Run:
```bash
printf "protocol=https\nhost=github.com\n" | git credential reject
```

This will remove the saved token for github.com.
After that, with the following command:
```bash
git push
```

Git will ask again:
```bash
Username:
Password:
```

In `Password:`, paste the copied new Git token.

### Method 2 — if a credential store is used
Check the helper:
```bash
git config --global credential.helper
```

If there:
```bash
store
```

then the Git token is stored in the file:
```bash
~/.git-credentials
```

You can remove the line from GitHub:
```bash
nano ~/.git-credentials
```

or delete the file entirely:
```bash
rm ~/.git-credentials
```

### Method 3 — check the remote (sometimes this is the cause)
Sometimes the Git token is hardcoded directly in the URL. Check:
```bash
git remote -v
```

Should be something like:
```bash
https://github.com/username/repo.git
```

If there's a Git token there, you need to remove it.

## 4. Securely backup git-recovery-codes

1. Download recovery codes from GitHub (to restore two-factor authentication).
2. Encrypt (AES-256) the git-recovery-codes.txt file:
```bash
ls -l git-recovery-codes.txt
chmod 600 git-recovery-codes.txt
gpg --symmetric --cipher-algo AES256 git-recovery-codes.txt
```

What will happen:
- You will be asked to enter a passphrase
- A file will be created:
```bash
git-recovery-codes.txt.gpg
```
The original file will remain (we'll delete it later).

3. Burn to a secure drive, such as a DVD:
```bash
# Example of burning an ISO to a DVD
wodim -v dev=/dev/sr0 -data git-recovery-codes.txt
```

4. Make sure the DVD is stored securely and access is restricted.

5. The same password is required for decryption.
```bash
gpg -d github-recovery-codes.txt.gpg
```

If the password is lost, ❌ it cannot be recovered (this is normal and correct).

## 5. Check GnuPG directories

```bash
# Check permissions on keys and configuration
ls -l ~/.gnupg
gpg --list-keys
gpg --list-secret-keys
```

Recommendations:
- The permissions should be 700 for the directory and 600 for the key files.
```bash
chmod 700 ~/.gnupg
chmod 600 ~/.gnupg/*
```

## 6. Changing the SSH key passphrase

* Run:
```bash
ssh-keygen -p -f ~/.ssh/id_ed25519
```

* Next:
  1. Enter the old passphrase (if any)
  2. Enter the new passphrase
  3. Confirm
The private key will remain the same, only the security will change.

* Check key permissions:
```bash
ls -ld ~/.ssh
ls -l ~/.ssh
```

* Expected:
  - ~/.ssh → drwx------
  - id_ed25519 → -rw-------
  - id_ed25519.pub → -rw-r--r--
  - authorized_keys → -rw-------

* If suddenly it’s not right, fix it:
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys
```

* Check via ssh-agent (optional)
```bash
ssh-add ~/.ssh/id_ed25519
```

* View loaded keys:
```bash
ssh-add -l
```

## 7. Working with branches

* View all local and remote branches:
```bash
git branch -a
```

* Merge changes:
```bash
git checkout main
git pull origin main # --rebase can be specified
git merge feature/name
```

* Revert changes to the last commit:
```bash
git restore <file>
git restore .
```

* Deleting local branches after merging:
```bash
git branch -d feature/name
```

## 8. Pushing and security

* Before pushing, ensure the branch is up-to-date:
```bash
git fetch origin
git status
git diff
```

* ⚠️ Don't use --force unless absolutely necessary
```bash
git push origin main
```

* If you need to force (very carefully):
```bash
git push --force-with-lease
```

## 9. Checking changes and integrity

- Checking commit hashes:
```bash
git log --oneline --graph --decorate
```

- Checking local integrity:
```bash
git fsck
```

## 10. Backing up local repositories

* Creating a local archive
```bash
tar -czf ~/backup-repository.tar.gz repository/
```

* Verify archive:
```bash
ls -lh ~/backup-repository.tar.gz
```

## 11. Tips

* Use a separate GitHub account for testing.
* Don't push sensitive data.
* Set up .gitignore for temporary files and tokens.
* Enable two-factor authentication.
* Scan directories and logs before deleting.

## 12. 🔑 Operating system password

* Use a strong user password for your Linux account.
* The password must be unique, sufficiently long, more than 10 characters, and randomly complex.
* Never use the root password for everyday work with Git or scripts.
* If you need to run scripts with `sudo`, make sure the command is safe:
  - Trust only trusted sources.
  - Read the script and check which commands are executed with root privileges.
    - This is especially important for the following commands: `rm`, `dd`, `mkfs`, `ln -sf`.
  - Do not copy `sudo` commands from unknown sources.
  - For testing, you can first run the script without `sudo` to check the output and behavior.
  - Use `sudo -l` to find out which commands your account can execute with root privileges.
  
> [!] If your password is compromised, it makes sense to back up the system or user profile only for the cleaned system.

## See also

- [Git-Brandmauer — GitHub](https://github.com/krashevski/git-brandmauer/blob/main/README.md)
- Repository management guidelines [REPOSITORY_STANDARDS_EN.md](README_REPOSITORY_STANDARDS_EN.md)
