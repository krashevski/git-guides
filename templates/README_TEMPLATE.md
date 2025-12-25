# Ubuntu Backup and Restore Kit

Backup kit for creating system backups before reinstalling Ubuntu and restoring the working system afterwards.

## Components

- `rsync-backup-v1.3.sh` — main backup script for users data
- `install-v1.3.sh` — installation and prerequisites check script
- `backup_kit_ubuntu-22.04-v1.3.sh` — backup of installed packages and Ubuntu repositories
- `rsync-restore-userdata-v1.3.sh` - script for restoring users data
- `restore_ubuntu-22.04-v1.3.sh` — script for restoring after system reinstall
- `README.md` — instructions in English
- `README_RU.md` — instructions in Russian
- `QUICKSTART.md` — short quick-start guide

## System requirements
 
- Ubuntu 22.04 (other versions are not guaranteed to work properly.)
- Packages:
  - `rsync`
  - `screen`
  - `tar`
- Access:
  - To the graphical terminal (for unpacking the archive and basic work).
  - To the local console **TTY3** (login under your user, enter login and password).
  - Superuser rights (`sudo`) in the graphical terminal for individual commands.

## Installation

```bash
tar -xvzf backup_kit_mirror_logs.tar.gz
cd backup_kit_mirror_logs
bash install.sh
```

> [i] install.sh is a wrapper for the install-v1.3.sh script and automatically checks for the presence of necessary packages.

## Quick Test (for verification only)

> [i] In the final version (for actual restoration after system reinstall) this section is recommended to be removed.
```bash
rsync-backup.sh profile1
cat /home/user/log/backup.log
```

> [i] `rsync-backup.sh` is a wrapper for the `rsync-backup-v1.3.sh` script and automatically checks for the presence of necessary packages.

## Script Verification

Run the script in dry-run mode (without making changes):
```bash
rsync-backup.sh profile1 --dry-run
```

This allows you to:
- Verify that screen sessions start and finish correctly
- Ensure logs are created in the correct location
- See which files would be copied without actually modifying anything

To test an error script (to see how the [ERROR] block works):
```bash
rsync-backup.sh custom /fake/path test --dry-run
```

## Usage

### Open the graphical terminal (GUI) and unpack the archive, if you haven't done so already:
```bash
tar -xvzf backup_kit_mirror_logs.tar.gz
cd backup_kit_mirror_logs
```

### Switch to the local console (TTY3):

Ctrl+Alt+F3

### Run the script:

```bash
rsync-backup.sh run
```

> [i] `rsync-backup.sh` is a wrapper for the `rsync-backup-v1.3.sh` script and automatically checks for the presence of necessary packages.

### "After completion, switch back to the graphical interface:

Ctrl+Alt+F1

### Backup a single profile:

```bash
rsync-backup.sh profile1
rsync-backup.sh profile2
rsync-backup.sh profile3
```

### Backup a custom directory:

```bash
rsync-backup.sh /path/to/directory
```

### Show help:

```bash
rsync-backup.sh -h
```

### Show version:

```bash
rsync-backup.sh -v
```

### Logs

All operations are recorded in:
```bash
cat /home/user/log/backup.log
```

## Recovery

After reinstalling Ubuntu, you can restore your saved data and package list.

### Switch to the GUI (Ctrl+Alt+F2), unpack the archive:

```bash
tar -xvzf backup_kit_mirror_logs.tar.gz
cd backup_kit_mirror_logs
```

### Run the recovery script:

```bash
bash restore_ubuntu-22.04-v1.3.sh
```

### Follow the script prompts:

- profiles and user data will be restored,
- the list of packages and repositories will be restored,
- if necessary, you will need to enter the password for `sudo`.

> [!] It is recommended to run the recovery script only on a clean installed system to avoid conflicts.

> [OK] After the script completes, the system will be ready for use.

## See Also

For quick start instructions and examples, refer to [QUICKSTART_TEMPLATE.md](QUICKSTART_TEMPLATE.md)
