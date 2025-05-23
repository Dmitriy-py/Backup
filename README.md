# Домашнее задание к занятию 3 «Резервное копирование»

## ` Климов Дмитрий  `

## Задание 1

1. Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup
2. Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
3. Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
На проверку направить скриншот с командой и результатом ее выполнения

## Ответ:

![Снимок экрана (1068)](https://github.com/user-attachments/assets/ec33d4d1-f979-4ca3-ad5b-0d6fe9049bf3)

![Снимок экрана (1069)](https://github.com/user-attachments/assets/9c6a93a7-3614-4f94-a618-363e0fca88e4)

![Снимок экрана (1070)](https://github.com/user-attachments/assets/8435e265-0064-4ca4-8502-8fd3df19cf85)


## Задание 2

1. Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
2. Резервная копия должна быть полностью зеркальной
3. Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
4. Резервная копия размещается локально, в директории /tmp/backup
На проверку направить файл crontab и скриншот с результатом работы утилиты.

## Ответ:

` /usr/local/bin/backup_script.sh `

```
#!/bin/bash

# Директория для резервной копии
BACKUP_DIR="/tmp/backup"

# Домашняя директория пользователя
HOME_DIR="/home/$USER"

# Лог-файл
LOG_FILE="/var/log/backup.log"

# Дата и время для записи в лог
TIMESTAMP=$(date +'%Y-%m-%d %H:%M:%S')

# Команда rsync для создания зеркальной резервной копии
RSYNC_COMMAND="rsync -av --delete --checksum --exclude '.*' $HOME_DIR/ $BACKUP_DIR"

# Выполнение резервного копирования
$RSYNC_COMMAND

# Проверка статуса выполнения rsync
if [ $? -eq 0 ]; then
  echo "$TIMESTAMP: Резервное копирование выполнено успешно." >> $LOG_FILE
else
  echo "$TIMESTAMP: Ошибка при выполнении резервного копирования." >> $LOG_FILE
fi


```

` crontab `

```
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
0 12 * * * /usr/local/bin/backup_script.sh


```

![Снимок экрана (1071)](https://github.com/user-attachments/assets/74e3d729-534d-4bc0-945c-6782503b84a1)

![Снимок экрана (1072)](https://github.com/user-attachments/assets/35080ca3-aef0-44b4-8b57-a1fe53c1f205)

![Снимок экрана (1073)](https://github.com/user-attachments/assets/d6e3449b-6c51-486f-b964-c29f1773a62f)



