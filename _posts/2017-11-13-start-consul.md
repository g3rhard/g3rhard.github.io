---
layout: post
title:  "Начало работы с Consul"
date:   2017-11-13 09:00:00 +0800
categories: consul 
---

*Несколько вольный перевод [статьи с DO](https://www.digitalocean.com/community/tutorials/an-introduction-to-using-consul-a-service-discovery-system-on-ubuntu-14-04)*

#### Введение

Что такое Consul? Consul - это распределенная система высокого уровня доступности для поиска сервисов и систем конфигурации. Она может быть использована для представления сервисов и нод в расширяемом интерфейсе, который в свою очередь позволяет клиентам всегда иметь последнюю информацию о состоянии инфраструктуры.

Что это значит своими словами? Это означает, что кроме предоставления сервиса DNS, Consul является неким healthcheck-сервисом, который позволяет, используя пять способов (script+interval, http+interval, tcp+interval, TTL, docker+interval), получать информацию о работающих сервисах. Более подробно о вариантах проверки с примерами в официальной документации [Consul - Checks](https://www.consul.io/docs/agent/checks.html).


### Дополнительные ссылки:

1. [Scott Lowe Blog - Quick intro to Consul](https://blog.scottlowe.org/2015/02/06/quick-intro-to-consul/)
2. [Ксакеп - Consul](https://xakep.ru/2016/04/18/consul/)
3. [EAX - Consul](http://eax.me/consul/)
