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
2. Узнать количество строк в таблице:
```
SELECT COUNT(1) FROM название_таблицы
# SQL-запрос с условием:
SELECT COUNT(1) FROM название_таблицы WHERE условие
```
3. Сделать дамп БД:
```
mysqldump -u USERNAME -p --single-transaction DB_NAME > DUMP_NAME.sql
```
4. Залить дамп БД:
```
mysql -u USERNAME -p DB_NAME < DUMP_NAME.sql
```
5. Узнать права пользователя:
```
show grants for 'USER'@'HOST';
```
6. Выполнить дамп и залить его на новый сервер, без промежуточного файла:
```
mysqldump -u USER -p'PASSWORD' --single-transaction DB_NAME | mysql -u USER -h HOST -p'PASSWORD' DB_NAME
```

На этом все.

### Дополнительные ссылки:
1. [mysql: полезные команды и настройки](https://proft.me/2011/07/19/mysql-poleznye-komandy-i-nastrojki/)
2. [mysqldump: 1044 Access denied when using LOCK TABLES](https://michaelrigart.be/mysqldump-1044-access-denied-using-lock-tables/)
3. [DO - Migrate MySQL](https://www.digitalocean.com/community/tutorials/how-to-migrate-a-mysql-database-between-two-servers)
4. [The MySQL SYS Schema in MySQL 5.7.7](https://mysqlserverteam.com/the-mysql-sys-schema-in-mysql-5-7-7/)