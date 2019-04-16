---
layout: post
title:  "Начало работы с Consul. Часть 1"
date:   2017-11-13 09:00:00 +0800
categories: consul 
---

***Несколько вольный перевод [статьи с DO](https://www.digitalocean.com/community/tutorials/an-introduction-to-using-consul-a-service-discovery-system-on-ubuntu-14-04)***

### Введение

Что такое Consul? Consul - это распределенная система высокого уровня доступности для поиска сервисов и систем конфигурации. Она может быть использована для представления сервисов и нод в расширяемом интерфейсе, который в свою очередь позволяет клиентам всегда иметь последнюю информацию о состоянии инфраструктуры.

Что это значит своими словами? Это означает, что кроме предоставления сервиса DNS, Consul является неким healthcheck-сервисом, который позволяет, используя пять способов (script+interval, http+interval, tcp+interval, TTL, docker+interval), получать информацию о работающих сервисах. Более подробно о вариантах проверки с примерами в официальной документации [Consul - Checks](https://www.consul.io/docs/agent/checks.html).

У Consul'a есть некоторые особенности, которые выделяют его среди других решений, например:
- Хранилище регистров типа ключ:значение;
- Встроенный healthchecker, который может подстраиваться под пользовательские решения, или встраиваться уже в широко распространенные;
- Придерживается [REST](https://ru.wikipedia.org/wiki/REST) стиля HTTP API для взаимодействия;
- "Может" в балансировку нагрузки;
- Поддерживает как single node установку, так и кластеризацию;
- Хорошо интегрируется с Docker.

### Установка Consul

Как уже говорилось раньше, Consul подходит под разные режимы установки, как stand alone, так и cluster mode. Для тестовых целей вполне подходит stand alone. Для полноценной, не тестовой, работы, конечно лучше выбирать режим, когда Consul будет находиться на нескольких нодах, что бы была некая избыточность данных. Когда такая схема запускается, есть много обязательных для выполнения пунктов: определиться с протоколом общения нод, выбрать лидера и создать работающий кластер.

Consul может быть установлен с помощью Docker, в этой записи будет использоваться именно этот способ. Запущу я его с помощью docker-compose, используя вот такой файл:

```
version: '3' # more about this at https://docs.docker.com/compose/compose-file/
services:
  consul:
    image: consul:latest # official and latest release of consul
    volumes:
      - server.json:/consul/config/server.json:ro # connect our config file for server
    ports:
      - "8400:8400"
      - "8500:8500" # consul ui - web interface
      - "8600:53/udp" # our other containers can use consul dns service
volumes:
  server.json:
```

На этом все.

### Дополнительные ссылки

1. [Scott Lowe Blog - Quick intro to Consul](https://blog.scottlowe.org/2015/02/06/quick-intro-to-consul/)
2. [Ксакеп - Consul](https://xakep.ru/2016/04/18/consul/)
3. [EAX - Consul](http://eax.me/consul/)
4. [Sreenivas Makam's Blog - Service Discovery with Consul](https://sreeninet.wordpress.com/2016/04/17/service-discovery-with-consul/)
