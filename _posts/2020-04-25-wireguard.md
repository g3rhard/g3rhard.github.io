---
layout: post
title:  "Настройка Wireguard для Orange Pi PC"
date:   2020-04-25 09:00:00 +0800
categories: nix cli
---

1. Настройка сети в Orange Pi:

```sh
# Обновляем /etc/sysctl.conf заменим строку
net.ipv4.ip_forward=1
sysctl -p
# Теперь расскажем нашему серверу, куда собственно направлять трафик, который прилетает на наш сервер, как на шлюз (вместо eth0 укажите тот интерфейс, которым шлюз смотрит в инет):
iptables -t nat -A POSTROUTING -o wg0 -j MASQUERADE
```

На этом все.

### Дополнительные ссылки

1. [Debian, nat. Раздаём интернет с debian. Шлюз на debian — часть 2](https://debian.pro/249)
2. [Armbian configuration utility](https://docs.armbian.com/User-Guide_Armbian-Config/)
