---
layout: post
title:  "Полезные однострочники - часть 2"
date:   2018-05-22 09:00:00 +0800
categories: nix cli
---

1. Список сервисов, которые слушают сетевые интерфейсы (не сокеты):
```sh
netstat -apn | grep LISTEN | grep -v unix
```
2. Как просмотреть строки с 65 по 70 в файле?
```sh
cat file.txt | sed -n 65,70p
```
3. Выводим вместе с grep номер строки в файле:
```sh
cat file.txt | grep -n "some text"
```
4. Как посмотреть, когда был установлен пакет в Debian?
```sh
grep "installed PKG_NAME" /var/log/dpkg.log
zgrep "installed PKG_NAME" /var/log/dpkg.log.*
```
5. Добавить пользователя в группу
```sh
usermod -a -G GROUP USER
```
6. Сгенерировать публичный ключ из приватного ключа
```sh
chmod 400 key.pem
ssh-keygen -y -f key.pem > key.pub
```
7. Сконвертировать mp3 в mp4, используя статичную картинку, как кадр
```sh
ffmpeg -loop_input -i cover.jpg -i soundtrack.mp3 -shortest -acodec copy output_video.mp4
```
8. Скопировать данные с одного хоста на другой, используя rsync
```sh
rsync --progress -avz -e ssh /directory/path/ user@host:/path/on/server/
rsync --progress -avz -e ssh user@host:/path/on/server/ /directory/path/
```
9. Синхронизация папок на сервере
```sh
rsync -avzh /FOLDER1/source /FOLDER2/
```
10. Показать IP адреса контейнеров Docker
```sh
{% raw %} docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(docker ps -aq){% endraw %}
```

На этом все.

### Дополнительные ссылки

1. [7 Tips – Tuning Command Line History in Bash](https://www.shellhacks.com/tune-command-line-history-bash/)
2. [КОПИРОВАНИЕ ДАННЫХ С ПОМОЩЬЮ RSYNC](https://www.baf.ru/2008/02/13/kopirovanie-dannyh-s-pomoshhju-rsync/)
3. [Bash One-Liners](http://www.bashoneliners.com/)
4. [Rsync (Remote Sync): 10 Practical Examples of Rsync Command in Linux](https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)
5. [SO - How to get a Docker container's IP address from the host?](https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host)
