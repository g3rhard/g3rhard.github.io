---
layout: post
title:  "Полезные однострочники - часть 3"
date:   2019-04-15 09:00:00 +0800
categories: nix cli
---

1. Загрузить файл в VLC (iOS,Apple TV, etc) используя curl:
```sh
curl -F files[]=@FILE_NAME http://VLC_IP/upload.json --progress-bar -o /dev/stdout
```
2. Удалить строку из файла, используя vi*
```sh
vim +NUMBERd +wq FILE_NAME
```
3. Получить идентификатор приложения в Mac OS:
```sh
osascript -e 'id of app "Telegram"'
```
4. Проверка целостности архивов (например бэкапов):
```sh
# tar.gz
tar -tvzf ARCHIVE.tar.gz >/dev/null && echo "Archive is good!"
# gz
gunzip -t ARCHIVE.gz && echo "Archive is good!"
```
5. Конвертирование HEIC изображений в JPG (используя ImageMagick):
```sh
mogrify -format jpg *.heic
```
6. Соединить несколько mp3 файлов (например, аудиокниги) в каталоге в один файл (при условии одинакового битрейта):
```sh
# Вариант 1
cd FOLDER
cat *.mp3 > ONE_FILE.mp3
# Вариант 2
cat file1.mp3 file2.mp3 file3.mp3 > ONE_FILE.mp3
```

На этом все.

## Дополнительные ссылки

1. [LOR - 11414851](https://www.linux.org.ru/forum/admin/11414851)
2. [Elegant way to remove offending key from known_hosts file](https://coderwall.com/p/xij9gq/elegant-way-to-remove-offending-key-from-known-hosts-file)
3. [Как проверить созданный с помощью tar архив](http://itman.in/tar-check-archive)
4. [Convert HEIC images to JPG](https://zwbetz.com/convert-heic-images-to-jpg)
