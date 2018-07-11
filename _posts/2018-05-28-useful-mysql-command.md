---
layout: post
title:  "Полезные команды MySQL"
date:   2018-05-28 09:00:00 +0800
categories: nix mysql
---

1. Удалить binlog до определенной даты:
```
PURGE BINARY LOGS BEFORE '2018-01-01 12:00:00';
```


На этом все.

### Дополнительные ссылки:
1. [mysql: полезные команды и настройки](https://proft.me/2011/07/19/mysql-poleznye-komandy-i-nastrojki/)
2. [mysqldump: 1044 Access denied when using LOCK TABLES](https://michaelrigart.be/mysqldump-1044-access-denied-using-lock-tables/)
3. [DO - Migrate MySQL](https://www.digitalocean.com/community/tutorials/how-to-migrate-a-mysql-database-between-two-servers)