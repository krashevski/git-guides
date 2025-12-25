# QUICKSTART — Резервное копирование и восстановление (Ubuntu 22.04)

Этот краткий гид поможет быстро использовать Backup Kit v1.3 и Rsync Backup.

---

## Резервное копирование пользовательских данных

Сохраняем домашние каталоги в `~/backups`:
```bash
rsync-backup.sh all
```

Для отдельного профиля:
```bash
rsync-backup.sh profile1
```

Для произвольного каталога:
```bash
rsync-backup.sh custom /путь/к/каталогу имя_копии
```
Все логи сохраняются в `/home/<имя_пользователя>/log/backup/YYYY-MM-DD.log`.

## Полная резервная копия системы (Backup Kit)

```bash
./backup_kit_ubuntu-22.04-v1.3.sh
```
Будет сохранено:
- список всех пакетов (`installed-packages.list`)
- список вручную установленных пакетов (`manual-packages.list`)
- репозитории APT (`sources.list и sources.list.d/`)
- ключи APT (`apt-keys.asc`)
- пользовательские данные (`rsync-backup`)
- пользовательские логи (`logs/`)

Архив создаётся в `/home/<имя_пользователя>/backups/backup_kit_ubuntu-22.04.tar.gz`.

## Восстановление после переустановки Ubuntu

Используйте скрипт восстановления после переустановки Ubuntu для восстановления пакетов, пользовательских данных и журналов.

> [i] Восстановление вручную установленных пакетов (рекомендуется)
```bash
RESTORE_PACKAGES=manual ./restore_ubuntu-22.04-v1.3.sh
```

> [i] Восстановление полного списка пакетов и пользовательских логов
```bash
RESTORE_PACKAGES=full RESTORE_LOGS=true ./restore_ubuntu-22.04-v1.3.sh
```

> [i] Только восстановление логов
```bash
RESTORE_PACKAGES=none RESTORE_LOGS=true ./restore_ubuntu-22.04-v1.3.sh
```

### Логи будут помещены в `~/backups/restored_logs/`.

Перед установкой полного списка пакетов рекомендуется выполнить:
```bash
sudo apt-get update
sudo apt-get -f install
```

## Примечания

> [i] Для корректной работы `rsync-backup` рекомендуется запускать в `screen`.

> [i] Все скрипты должны находиться в одной папке или быть в `$HOME/bin`.

См. полную документацию в [README_TEMPLATE_RU.md](README_TEMPLATE_RU.md).


