# Quick Start — Backup and Restore (Ubuntu 22.04)

This quick guide will help you quickly use Backup Kit v1.3 and Rsync Backup.

---

## Backing up user data

Saving home directories in `~/backups`:
```bash
rsync-backup.sh all
```

For a separate profile:
```bash
rsync-backup.sh profile1
```

For a custom directory:
```bash
rsync-backup.sh custom /path/to/directory backup_name
```
All logs are saved in `/home/<username>/log/backup/YYYY-MM-DD.log`.

## Full system backup (Backup Kit)

```bash
./backup_kit_ubuntu-22.04-v1.3.sh
```
Will save:
- list of all packages (`installed-packages.list`)
- list of manually installed packages (`manual-packages.list`)
- APT repositories (`sources.list` and `sources.list.d/`)
- APT keys (`apt-keys.asc`)
- user data (`rsync-backup/`)
- user logs (`logs/`)


Archive is created in `/home/<username>/backups/backup_kit_ubuntu-22.04.tar.gz`.

## Recovering from Ubuntu reinstallation

Use the recovery script after reinstalling Ubuntu to restore packages, user data, and logs.

> [i] Restore manually installed packages (recommended)
```bash
RESTORE_PACKAGES=manual ./restore_ubuntu-22.04-v1.3.sh
```

> [i] Restore full package list and user logs
```bash
RESTORE_PACKAGES=full RESTORE_LOGS=true ./restore_ubuntu-22.04-v1.3.sh
```

> [i] Restore logs only
```bash
RESTORE_PACKAGES=none RESTORE_LOGS=true ./restore_ubuntu-22.04-v1.3.sh
```

### Logs will be placed in `~/backups/restored_logs/`.

Before installing the full list of packages, it is recommended to run:
```bash
sudo apt-get update
sudo apt-get -f install
```

## Notes

> [i] For correct operation of `rsync-backup`, it is recommended to run in `screen`.

> [i] All scripts must be in the same folder or be in `$HOME/bin`.

See full documentation in [README.md](README.md).
