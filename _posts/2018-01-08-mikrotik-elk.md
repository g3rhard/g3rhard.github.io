---
layout: post
title:  "Настраиваем централизованный сбор логов Mikrotik используя ELK"
date:   2018-01-08 09:00:00 +0800
categories: mikrotik
---
***По мотивам [YouTube - Централизованный сбор логов MikroTik c помощью ELK](https://www.youtube.com/watch?v=Lgyp6T-FIqU)***

### Настраиваем сервер

Я буду использовать свой форк [ELK](https://github.com/g3rhard/docker-elk) основанного на docker-compose. Конечно, ничего не мешает использовать свои варианты.

Для начала, мы добавим новый файл конфигурации для Logstash, который будет слушать порт 5140 именно для логов Mikrotik. Почему так? Потому как логи Mikrotik несколько отличаются от стандартных записей Syslog, и что бы мы могли отличать их по меткам в Kibana. Файл конфигурации будет доступен здесь. Для начала - проясним пару моментов в файле конфигурации:

```json
match => {
    "message" => [
        "%{WORD:syslogtype} dns: query from %{IP:clientip}: #%{NUMBER:requestid} %{URIHOST:nsname}. %{WORD:nstype}",
        ...
    ]
}
``` 

В этом месте, мы как раз таки определяем, какие записи из всех, что нам посылает Mikrotik мы будем разбирать и в каком формате. Как видно чуть ранее, мы используем плагин фильтра под названием Grok, он помогает из мешанины текста выцеплять необходимые нам данные и уже разборчиво, в формате JSON отправлять их в Elasticsearch.

### Настройка Mikrotik

- Открываем на панели пункт меню System - Logging;
- В появившемся окне на вкладке Action правим действие "remote" или добавляем свое;
- В этом же окне на вкладке Rules добавляем свое правило, какие логи мы хотим отправлять в Elasticsearch.


На этом все.

### Дополнительные ссылки:
1. [Mikrotik - Wiki](https://wiki.mikrotik.com/)
2. [Elastic - GROK](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)
3. [Grok Debug Test App](http://grokdebug.herokuapp.com)
4. [Logz.io - GROK]((https://logz.io/blog/logstash-grok/)
