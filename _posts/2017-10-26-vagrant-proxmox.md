---
layout: post
title:  "Сборка плагина vagrant-proxmox"
date:   2017-10-26 09:00:00 +0800
categories: vagrant 
---

Короткая заметка об установке плагина vagrant-proxmox, поскольку выполнение команды `proxmox plugin install vagrant-proxmox` устанавливает старую версию плагина.

```sh
apt install -y ruby-dev build-essential zlib1g-dev
git clone https://github.com/lehn-etracker/vagrant-proxmox
cd vagrant-proxmox/
bundle install
```

Если будет появляться ошибка об отсутствии rake, то надо создать символическую ссылку с бинарными файлами
```sh
mkdir -p /usr/share/rubygems-integration/all/gems/rake-10.5.0/bin
ln -s /usr/bin/rake /usr/share/rubygems-integration/all/gems/rake-10.5.0/bin/
```

И продолжаем:
```sh
bundle install
vagrant plugin install vagrant-proxmox-*.gem
```

Бонус:

Для использования vagrant вместе с самоподписанным сертификатом proxmox необходимо проделать следующий трюк:
```sh
cat /etc/pve/pve-root-ca.pem /etc/pve/local/pve-ssl.pem > /tmp/bundle.pem
SSL_CERT_FILE="/tmp/bundle.pem" vagrant up --provider=proxmox
```

На этом все.