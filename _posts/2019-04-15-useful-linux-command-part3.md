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

На этом все.

### Дополнительные ссылки

1. [LOR - 11414851](https://www.linux.org.ru/forum/admin/11414851)
2. [Elegant way to remove offending key from known_hosts file](https://coderwall.com/p/xij9gq/elegant-way-to-remove-offending-key-from-known-hosts-file)
