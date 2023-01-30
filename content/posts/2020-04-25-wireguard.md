---
layout: post
title:  "Настройка Wireguard для Orange Pi PC"
date:   2020-04-25 09:00:00 +0800
categories: nix cli
---

Не так давно, решил попробовать вместо L2TP VPN относительно новый (старый) протокол WireGuard.

*1*. На сервере, на котором будем использовать Wireguard (я использовал образ Docker linuxserver/docker-wireguard):

  * Можно использовать следующий docker-compose:

    {% gist 6b735868b6b9a6f75fee5ea8371f428f %}

  * Генерируем ключи:

    ```sh
    docker-compose exec wireguard bash
    wg genkey | tee privatekey_host | wg pubkey > publickey_host
    wg genkey | tee privatekey_peer | wg pubkey > publickey_peer
    ```

  * Генерируем конфиг сервера:

    ```
    cat /var/data/docker/wireguard/wg0.conf
    [Interface]
    Address = 10.13.13.1
    MTU = 1460
    ListenPort = SERVER_PORT
    PrivateKey = privatekey_host
    PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE; iptables -A FORWARD -i eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
    PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE; iptables -D FORWARD -i eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT

    [Peer]
    PublicKey = publickey_peer
    AllowedIPs = 10.13.13.2/32
    ```

  * Перезапускаем контейнер с Wireguard:

    ```sh
    doocker-compose restart
    ```

*2*. Настройка сети в Orange Pi:

  * Настраиваем параметры ядра для форвардинга пакетов:

    ```sh
    # Обновляем /etc/sysctl.conf заменим строку
    net.ipv4.ip_forward=1
    sysctl -p
    ```

  * Устанавливаем статический IP, это можно сделать либо на роутере (в настройках DHCP сервера), либо используя утилиту armbian-config на OrangePI


*3*. Устанавливаем пакет Wireguard на OrangePI (пакет представлен в оф. репозиториях Debian):

  ```sh
  sudo apt install wireguard
  ```

*4*. Используя полученные с сервера данные, создаем конфигурацию для клиента WireGuard на OrangePI:

  ```sh
  cat /etc/wireguard/wg0.conf
  [Interface]
  PrivateKey = privatekey_peer
  Address = 10.13.13.2/32 # IP из подсети, настроенной на сервере WireGuard
  MTU = 1360 # К выбору MTU нужно подойти внимательно
  # Правила ниже не обязательны, но если VPN будет работать не стабильно, лучше иметь возможность доступа в интернет, не меняя gateway на клиентских устройствах
  PostUp = ip route add SERVER_IP/32 via ROUTER_GATEWAY dev eth0; iptables -A FORWARD -i wg0 -m state --state RELATED,ESTABLISHED -j ACCEPT; iptables -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu; iptables -t mangle -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
  PostDown = ip route del SERVER_IP/32 via ROUTER_GATEWAY dev eth0; iptables -D FORWARD -i wg0 -m state --state RELATED,ESTABLISHED -j ACCEPT; iptables -D FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu; iptables -t mangle -D FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu

  [Peer]
  PublicKey = publickey_host
  AllowedIPs = 0.0.0.0/0
  Endpoint = SERVER_IP:SERVER_PORT # Внешний IP сервера
  PersistentKeepalive = 25
  ```

*5*. Если все будет корректно настроено, то соединение поднимется по команде (на OrangePI):

  ```sh
  sudo service wg-quick@wg0 start
  sudo wg show
  ```

За время использования была замечена большая стабильность соединения, более высокая скорость работы (по сравнению с L2TP), и меньшая нагрузка на CPU.

На этом все.

## Дополнительные ссылки

1. [Debian, nat. Раздаём интернет с debian. Шлюз на debian — часть 2](https://debian.pro/249)
2. [Armbian configuration utility](https://docs.armbian.com/User-Guide_Armbian-Config/)
3. [Github - linuxserver/wireguard](https://github.com/linuxserver/docker-wireguard)
4. [iVPN - Wireguard kill-switch](https://www.ivpn.net/knowledgebase/238/Linux---WireGuard-Kill-switch.html)
