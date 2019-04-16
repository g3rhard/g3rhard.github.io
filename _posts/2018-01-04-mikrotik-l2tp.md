---
layout: post
title:  "Выходим в интернет за пределами страны (Mikrotik + L2TP)"
date:   2018-01-04 09:00:00 +0800
categories: mikrotik vpn
---

### Настраиваем сервер

Все что нужно - это использовать уже готовый скрипт от вот этих ребят - [hwdsl2/setup-ipsec-vpn](https://github.com/hwdsl2/setup-ipsec-vpn):

```sh
wget https://git.io/vpnsetup -O vpnsetup.sh && sudo \
VPN_IPSEC_PSK='IPSEC_PSK' \
VPN_USER='VPN_USER' \
VPN_PASSWORD='VPN_PASSWORD' sh vpnsetup.sh
```

### Настраиваем Mikrotik

Создаем новое соединение L2TP

```sh
/interface l2tp-client
add allow=chap,mschap2 connect-to=SERVER_IP disabled=no ipsec-secret=\
    "IPSEC_PSK" max-mru=1280 max-mtu=1280 name=l2tp-out1 password=\
    VPN_PASSWORD use-ipsec=yes user=VPN_USER
```

Добавляем DNS провайдеров (Google, Quad9, OpenDNS) в систему

```sh
/ip dns set allow-remote-requests=yes servers=9.9.9.9 # Quad9
```

Если у вас PPPoE соединение и провайдер предоставляет свои DNS, то необходимо в интерфейсе Winbox снять галочку с пункта "Use Peer DNS" в настройках PPPoE соединения.

Заворачиваем трафик DNS до нашего DNS-провайдера

```sh
/ip route add distance=1 dst-address=9.9.9.9/32 gateway=l2tp-out1
```

Добавляем masquerade для трафика

```sh
/ip firewall nat add chain=srcnat action=masquerade out-interface=l2tp-out1
```

Добавляем маркировку для трафика, который будет находится в списке AnotherGWList

```sh
/ip firewall mangle add chain=prerouting src-address=192.168.0.0/24 dst-address-list=AnotherGWList action=mark-routing new-routing-mark=AnotherGWRoute
```

Добавляем маршрут для промаркированного трафика через наш VPN

```sh
/ip route add distance=1 routing-mark=AnotherGWRoute gateway=l2tp-out1
```

Сбрасываем кэш DNS

```sh
/ip dns cache flush
```

И добавляем нужные нам ресурсы в список AnotherGWList

```sh
/ip firewall address-list add address=www.linkedin.com list=AnotherGWList
```

На этом все.

### Дополнительные ссылки

1. [Habrahabr - Выходим в интернет за пределами РФ](https://habrahabr.ru/post/337426/)
2. [Habrahabr - Настройка OpenVPN в связке Mikrotik/Ubuntu](https://habrahabr.ru/post/227767/)
3. [Mikrotik - Wiki](https://wiki.mikrotik.com/)
4. [GitHub - hwdsl2/setup-ipsec-vpn](https://github.com/hwdsl2/setup-ipsec-vpn)
