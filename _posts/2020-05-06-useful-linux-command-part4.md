---
layout: post
title:  "Полезные однострочники - часть 4"
date:   2020-05-06 09:00:00 +0800
categories: nix cli
---

*1*. Когда нужно разделить развороты внутри файла PDF

  ```sh
  # Разделить по вертикали
  mutool poster -x 2 source.pdf out.pdf
  # Разделить по горизонтали
  mutool poster -y 2 source.pdf out.pdf
  ```

*2*. Обновление пакетов pip3

  ```sh
  pip3 list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip3 install -U
  ```

*3*. Поиск источника SYN атаки

  ```sh
  netstat -n -p | grep SYN_RECV | wc -l
  netstat -n -p | grep SYN_RECV | sort –u
  netstat -n -p | grep SYN_REC | awk '{print $5}' | awk -F: '{print $1}'
  ```

На этом все.

### Дополнительные ссылки

1. [LOR - 12967886](https://www.linux.org.ru/forum/talks/12967886)
2. [SO - How to upgrade all Python packages with pip?](https://stackoverflow.com/questions/2720014/how-to-upgrade-all-python-packages-with-pip)
3. [VPS.UA - Как посмотреть количество подключений к серверу](https://vps.ua/wiki/view-connections-server/)
