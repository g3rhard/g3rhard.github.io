---
layout: post
title:  "Полезные трюки при работе в Linux"
date:   2018-10-23 09:00:00 +0800
categories: linux tricks cli
---

1. Повторить последнюю команду (например, с sudo):
```sh
$ command
$ sudo !!
sudo command
```
2. Взять все аргументы/последний аргумент из предыдущей команды:
```sh
$ cd /home/user/foo
$ mkdir !*
mkdir /home/user/foo
...
$ ls -la /etc/hosts
$ vi !$
vi /etc/hosts
```
3. Удаляем сервер из .ssh/known_hosts
```sh
ssh-keygen -R SERVER_NAME_OR_IP
```
4. Проверить доступность порта с помощью nc:
```sh
nc -v -z SERVER_NAME_OR_IP PORT
```
5. Проверяем, чем занято место на диске с помощью ncdu:
```sh
# Сохраняем отчет в файл, для последующего просмотра
ncdu -o /tmp/ncdu.results /
# Открываем отчет
ncdu -f /tmp/ncdu.results
```

На этом все.

### Дополнительные ссылки
1. [https://xakep.ru/2017/05/18/cli-console-tips/](https://xakep.ru/2017/05/18/cli-console-tips/)
2. [WIREHIVE - 5 top tips for reviewing your Postfix mail queue](https://www.wirehive.com/thoughts/5-top-tips-reviewing-postfix-mail-queue/)
3. [Анализ использования диска при помощи ncdu](http://ashep.org/2013/analiz-ispolzovaniya-diska-pri-pomoshhi-ncdu/)
