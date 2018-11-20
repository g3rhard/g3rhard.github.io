---
layout: post
title:  "Полезные трюки при работе в Linux"
date:   2018-10-23 09:00:00 +0800
categories: linux tricks
---

1. Повторить последнюю команду (например, с sudo):
```
$ command
$ sudo !!
sudo command
```
2. Взять аргумент из предыдущей команды:
```
$ cd /home/user/foo
$ mkdir !*
mkdir /home/user/foo
```


На этом все.

### Дополнительные ссылки:
1. [https://xakep.ru/2017/05/18/cli-console-tips/](https://xakep.ru/2017/05/18/cli-console-tips/)
2. [WIREHIVE - 5 top tips for reviewing your Postfix mail queue](https://www.wirehive.com/thoughts/5-top-tips-reviewing-postfix-mail-queue/)