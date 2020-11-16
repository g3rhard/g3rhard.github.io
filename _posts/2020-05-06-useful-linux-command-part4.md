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

*4*. Проверка скорости загрузки сайта, используя curl

  ```sh
  curl -s -w '\nTesting Website Response Time for: %{url_effective}\n\nLookup Time:\t\t%{time_namelookup}\nConnect Time:\t\t%{time_connect}\nPre-transfer Time:\t%{time_pretransfer}\nStart-transfer Time:\t%{time_starttransfer}\n\nTotal Time:\t\t%{time_total}\n' -o /dev/null https://SITE_URL
  ```

*5*. Генерация known_hosts файла

  ```sh
  ssh-keygen -R [hostname]
  ssh-keygen -R [ip_address]
  ssh-keygen -R [hostname],[ip_address]
  ssh-keyscan -H [hostname],[ip_address] >> ~/.ssh/known_hosts
  ssh-keyscan -H [ip_address] >> ~/.ssh/known_hosts
  ssh-keyscan -H [hostname] >> ~/.ssh/known_hosts
  ```

На этом все.

### Дополнительные ссылки

1. [LOR - 12967886](https://www.linux.org.ru/forum/talks/12967886)
2. [SO - How to upgrade all Python packages with pip?](https://stackoverflow.com/questions/2720014/how-to-upgrade-all-python-packages-with-pip)
3. [VPS.UA - Как посмотреть количество подключений к серверу](https://vps.ua/wiki/view-connections-server/)
4. [SF - Can I automatically add a new host to known_hosts?](https://serverfault.com/questions/132970/can-i-automatically-add-a-new-host-to-known-hosts)
