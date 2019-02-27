---
layout: post
title:  "Небольшая заметка по вводу nix машин в домен Active Directory"
date:   2018-01-22 09:00:00 +0800
categories: nix win ad
---

Не так давно возникла необходимость входить в домен Active Directory, который работает под управление Windows Server 2016 на машинах под управлением Ubuntu 16.04, и так как способ, описанный в Ubuntu Wiki уже надоел, было решено поискать новый, более удобный способ.

Итак, относительно новый игрок на рынке, в котором за время тестирования не было выявлено косяков - Power Broker Identity Server. В целом, его использование - сплошные плюсы, единственный замеченный минус - необходимость перезагрузки машины, после входа в домен (как заверяет конфигуратор - это делается для лучшей работы).

Установка (PPA):

```
wget -O - http://repo.pbis.beyondtrust.com/apt/RPM-GPG-KEY-pbis|sudo apt-key add - 
sudo wget -O /etc/apt/sources.list.d/pbiso.list http://repo.pbis.beyondtrust.com/apt/pbiso.list 
sudo apt-get update
sudo apt-get install pbis-open
```

Плюсы:
- Правильная настройка множества файлов, связанных с авторизацией;
- Скорость настройки;
- Малое количество или практически полное отсутствие зависимостей;
- Широкий [список поддерживаемых дистрибутивов](https://www.beyondtrust.com/wp-content/uploads/documentation-pbis-supported-platforms-full-list.pdf)

Минусы:
- Это не свободный продукт, коммерческий, есть различия в версиях;
- На сайте производителя отсутствуют сравнение версий и немного неинформативна разница между версиями;

Нюансы настройки:

- Использование sh для пользователей по умолчанию - легко меняется на необходимый shell:

```
/opt/pbis/bin/config LoginShellTemplate /bin/bash
```

Резюме:

В целом продукт понравился - как результат достаточно долгой разработки - работает хорошо и на тестовых машинах не было замечено сбоев при входе в систему.

На этом все.

### Дополнительные ссылки:
1. [Power Broker Identity Services Open](https://www.beyondtrust.com/products/powerbroker-identity-services-open/)
2. [Ubuntu Wiki - Ввод в домен Windows](http://help.ubuntu.ru/wiki/ввод_в_домен_windows)
3. [PBIS Open - Wiki](https://github.com/BeyondTrust/pbis-open/wiki)
4. [Power Broker и другие средства авторизации - Tux.in.ua](https://www.tux.in.ua/articles/3117)