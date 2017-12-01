---
layout: post
title:  "Как расстаться с net-tools"
date:   2017-12-01 09:00:00 +0800
categories: new hack 
---

Небольшая заметка в качестве напоминания, что бы перестать использовать ifconfig и часть утилит, входящих в пакет net-tools.

Вообще пакет net-tools содержит и другие важные утилиты:

```
/bin/netstat
/sbin/ifconfig
/sbin/ipmaddr
/sbin/iptunnel
/sbin/mii-tool
/sbin/nameif
/sbin/plipconfig
/sbin/rarp
/sbin/route
/sbin/slattach
/usr/sbin/arp
```

Ну и самым популярным из них для меня долгое время являлся ifconfig. Однако, прогресс не стоит на месте и появилась новая утилита ip.
Синтаксис ее достаточно прост и понятен, присутствуют сокращения.
Почему он лучше чем старый ifconfig? Есть много разных причин, и одни из них:
* Показ адресов тунельных (tap*, tun*) интерфейсов;
* В одной утилите находится функционал ifconfig, route;
* В свежей centos7-minimal ifconfig по умолчанию отсутствует.

Эти причин достаточно, что бы захотеть научится новым штукам. Итак, приступим:

1. Самое простое - показать IP адреса на интерфейсах:
```sh
ip a # показать все адреса на всех интерфейсах 
ip -4 a # показать интерфейсы только с IPv4 адресами
ip -6 a # показать интерфейсы только с IPv6 адресами
```
2. Назначить/удалить c интерфейса IP адрес
```sh
ip a [add/del] {ip_addr/mask} dev {interface} # пример: 
ip a add 10.0.0.1/24 dev eth0 # добавить адрес на интерфейс eth0
ip a del 10.0.0.1/24 dev eth0 # удалить адрес с интерфеса eth0
```
3. Настройка маршрутизации
```sh
ip r # показать таблицу маршрутизации
ip r add {network/mask} via {gatewayip} # элементарно, как и обычное выполнение route
```

На этом все.

*По мотивам [статьи на Habrahabr](https://habrahabr.ru/post/320278/).*

### Дополнительные ссылки:
[Настройка сети в Linux](https://losst.ru/nastrojka-seti-v-linux)

[Wikipedia - IP (утилита)](https://ru.wikipedia.org/wiki/Ip_(%D1%83%D1%82%D0%B8%D0%BB%D0%B8%D1%82%D0%B0_Unix))

[Настройка сети в Debian](http://debian-help.ru/articles/nastroika-seti-s-pomoschyu-utility-ip-v-debian-linux/)