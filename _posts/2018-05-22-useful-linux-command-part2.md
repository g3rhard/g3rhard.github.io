---
layout: post
title:  "Полезные однострочники - часть 2"
date:   2018-05-22 09:00:00 +0800
categories: nix aws
---

1. Список сервисов, которые слушают сетевые интерфейсы (не сокеты):
```
netstat -apn | grep LISTEN | grep -v unix
```
2. Как просмотреть строки с 65 по 70 в файле?
```
cat file.txt | sed -n 65,70p
```
3. Выводим вместе с grep номер строки в файле:
```
cat file.txt | grep -n "some text"
```
4. Как посмотреть, когда был установлен пакет в Debian?
```
grep "installed PKG_NAME" /var/log/dpkg.log
zgrep "installed PKG_NAME" /var/log/dpkg.log.*
```
5. Добавить пользователя в группу
```
usermod -a -G GROUP USER
```
6. Сгенерировать публичный ключ из приватного ключа
```
chmod 400 key.pem
ssh-keygen -y -f key.pem > key.pub
```
7. Сконвертировать mp3 в mp4, используя статичную картинку, как кадр
```
ffmpeg -loop_input -i cover.jpg -i soundtrack.mp3 -shortest -acodec copy output_video.mp4
```
8. Скопировать данные с одного хоста на другой, используя rsync
```
rsync --progress -avz -e ssh /directory/path/ user@host:/path/on/server/
rsync --progress -avz -e ssh user@host:/path/on/server/ /directory/path/
```
9. Синхронизация папок на сервере
```
rsync -avzh /FOLDER1/source /FOLDER2/
```

На этом все.

### Дополнительные ссылки:
1. [7 Tips – Tuning Command Line History in Bash](https://www.shellhacks.com/tune-command-line-history-bash/)
2. [КОПИРОВАНИЕ ДАННЫХ С ПОМОЩЬЮ RSYNC](https://www.baf.ru/2008/02/13/kopirovanie-dannyh-s-pomoshhju-rsync/)
3. [Bash One-Liners](http://www.bashoneliners.com/)
4. [Rsync (Remote Sync): 10 Practical Examples of Rsync Command in Linux](https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)