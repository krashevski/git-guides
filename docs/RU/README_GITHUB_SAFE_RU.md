# 🔒 Безопасная работа с GitHub

[🇬🇧 English](../EN/README_GITHUB_SAFE_EN.md) | [🇷🇺 Русский](README_GITHUB_SAFE_RU.md)

Цель: дать безопасную методику работы с локальными репозиториями и удалёнными сервисами, минимизировать риск утечки токенов, случайного форс-пуша и других ошибок.

## 1. Настройка локального репозитория

### Клонируем репозиторий
```bash
git clone https://github.com/username/repository.git
cd repository
```

### Проверяем ветку по умолчанию
```bash
git branch
git status
```

Рекомендации:
- Никогда не работайте с root-учётной записью.
- Всегда проверяйте текущую ветку перед коммитом.
- Создавайте feature-ветки для новых изменений:

```bash
git checkout -b feature/название
```

## 2. Безопасное хранение Git токена

### Использование переменной окружения (только для текущей сессии):
```bash
export GIT_TOKEN="ваш_токен_сюда"
```
Рекомендации:
- Никогда не храните Git токен в репозитории (.env, .bashrc, README и т.п.).
- После окончания работы удалите переменную:
```bash
unset GIT_TOKEN
```

### Никогда не сохраняйте Git токен в репозитории:
```bash
# ❌ НЕ делать
echo "GIT_TOKEN=..." >> .env
```

### Использование файла .git-credentials с ограничением прав доступа:
```bash
git config --global credential.helper store
chmod 600 ~/.git-credentials
```

> [i] В этом случае Git токен будет храниться локально, защищённый правами пользователя.

## 3. Обновление Git токена с удалением

### Создать новый токен в GitHub
1. Откройте настройки:
```bash
https://github.com/settings/tokens
```

2. Перейдите:
Settings → Developer settings → Personal access tokens.

3. Нажмите:
Generate new token (или Regenerate token для существующего).

4. Укажите:
- срок действия (Expiration)
- необходимые права (например repo, workflow и т.д.)
- Нажмите Generate token.
- Скопируйте токен — GitHub покажет его только один раз.

### Способ 1 — удалить credential для GitHub (правильно)
Выполните:
```bash 
printf "protocol=https\nhost=github.com\n" | git credential reject
```

Это удалит сохранённый токен для github.com.
После этого при следующей команде:
```bash
git push
```

Git снова спросит:
```bash
Username:
Password:
```

В `Password:` вставьте скопированный новый Git токен.

### Способ 2 — если используется credential store
Посмотрите helper:
```bash
git config --global credential.helper
```

Если там:
```bash
store
```

тогда Git токен хранится в файле:
```bash
~/.git-credentials
```

Можно удалить строку с GitHub:
```bash
nano ~/.git-credentials
```

или полностью удалить файл:
```bash
rm ~/.git-credentials
```

### Способ 3 — проверить remote (иногда причина здесь)
Иногда Git токен прописан прямо в URL. Проверьте:
```bash
git remote -v
```

должно быть примерно:
```bash
https://github.com/username/repo.git
```

Если там есть Git токен — его нужно удалить.

## 4. Безопасное резервное копирование git-recovery-codes

1. Скачайте Recovery Codes с GitHub (для восстановления двухфакторной аутентификации).
2. Зашифруйте (AES-256) файл git-recovery-codes.txt:
```bash
ls -l git-recovery-codes.txt
chmod 600 git-recovery-codes.txt
gpg --symmetric --cipher-algo AES256 git-recovery-codes.txt
```

Что произойдёт:
- вас попросят ввести парольную фразу
- будет создан файл:
```bash
git-recovery-codes.txt.gpg
```
исходный файл останется (удалим позже).

3. Запишите на защищённый носитель, например DVD:
```bash
# Пример записи ISO на DVD
wodim -v dev=/dev/sr0 -data git-recovery-codes.txt
```

4. Убедитесь, что DVD надёжно хранится и доступ к нему ограничен.

5. Для расшифровки нужен тот же пароль
```bash
gpg -d github-recovery-codes.txt.gpg
```

Если пароль утерян — ❌ восстановить невозможно (это нормально и правильно).

## 5. Проверка каталогов GnuPG

```bash
# Проверка прав на ключи и конфигурацию
ls -l ~/.gnupg
gpg --list-keys
gpg --list-secret-keys
```

Рекомендации:
- Должны быть права 700 для каталога и 600 для файлов ключей.
```bash
chmod 700 ~/.gnupg
chmod 600 ~/.gnupg/*
```

## 6. Смена секретной фразы (passphrase) для SSH ключа

* Выполните:
```bash
ssh-keygen -p -f ~/.ssh/id_ed25519
```

* Далее:
  1. Введите старую passphrase (если была)
  2. Введите новую passphrase
  3. Подтвердите
Приватный ключ останется тем же, изменится только защита.

* Проверьте права на ключи:
```bash
ls -ld ~/.ssh
ls -l ~/.ssh
```

* Ожидаемо:
  - ~/.ssh → drwx------
  - id_ed25519 → -rw-------
  - id_ed25519.pub → -rw-r--r--
  - authorized_keys → -rw-------

* Если вдруг не так — исправить:
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys
```

* Проверка через ssh-agent (опционально)
```bash
ssh-add ~/.ssh/id_ed25519
```

* Посмотреть загруженные ключи:
```bash
ssh-add -l
```

## 7. Работа с ветками

* Просмотр всех локальных и удалённых веток:
```bash
git branch -a
```

* Слияние изменений:
```bash
git checkout main
git pull origin main  # можно указать --rebase
git merge feature/название
```

* Отмена изменений до последнего коммита:
```bash
git restore <файл>
git restore .
```

* Удаление локальных веток после слияния:
```bash
git branch -d feature/название
```

## 8. Push и безопасность

* Перед пушем убедитесь, что ветка актуальна:
```bash
git fetch origin
git status
git diff
```

* ⚠️ Не использовать --force без крайней необходимости
```bash
git push origin main
```

* Если нужно форсировать (очень осторожно):
```bash
git push --force-with-lease
```

## 9. Проверка изменений и целостности

- Проверка хешей коммитов:
```bash
git log --oneline --graph --decorate
```

- Проверка локальной целостности:
```bash
git fsck
```

## 10. Резервные копии локальных репозиториев

* Создание локального архива
```bash
tar -czf ~/backup-repository.tar.gz repository/
```

* Проверка архива:
```bash
ls -lh ~/backup-repository.tar.gz
```

## 11. Советы

* Используйте отдельный аккаунт GitHub для тестов.
* Не пушьте секретные данные.
* Настройте .gitignore для временных файлов и токенов.
* Включите двухфакторную аутентификацию.
* Проверяйте каталоги и логи перед удалением.

## 11. 🔑 Пароль операционной системы

* Используйте **надёжный пароль пользователя** для своей учётной записи в Linux.
* Пароль должен быть уникальным, достаточной длины более 10 символов и с случайной сложностью.
* Никогда не используйте root-пароль для повседневной работы с Git или скриптами.
* При необходимости запуска скриптов с `sudo` убедитесь, что команда безопасна:
  - Доверяйте только проверенным источникам.
  - Читайте скрипт и проверяйте, какие команды выполняются с root-правами.
    - Особенно важно для команд: `rm`, `dd`, `mkfs`, `ln -sf`.
  - Не копируйте команды `sudo` из неизвестных источников.
  - Для тестирования сначала можно запускать скрипт **без `sudo`**, чтобы проверить вывод и поведение.
  - Используйте `sudo -l`, чтобы узнать, какие команды ваша учётная запись может выполнять с root-правами.
  
> [!] При сомнениях в пароле резервную копию системы или профиля пользователя имеет смысл создавать только для очищенной системы.

## Смотри также

- [Git-Brandmauer — безопасная работа с GitHub](https://github.com/krashevski/git-brandmauer/blob/main/docs/RU/README_RU.md)
- Руководящие принципы по управлению репозиторием [REPOSITORY_STANDARDS_RU.md](README_REPOSITORY_STANDARDS_RU.md)
