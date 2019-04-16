---
layout: post
title:  "Полезные команды при работе с Docker"
date:   2018-09-11 09:00:00 +0800
categories: docker cli
---

1. Удалить неиспользуемые контейнеры (старые версии):
```sh
docker rmi $(docker images -q -f dangling=true)
docker rmi $(docker images | grep "^<none>" | awk '{print $3}')
```
2. Запустить shell внутри контейнера:
```sh
docker exec -it CONTAINER_NAME /bin/bash
```
3. Посмотреть список всех контейнеров:
```sh
docker ps --all
```
4. [Обновить опции контейнера docker](https://docs.docker.com/engine/reference/commandline/update/)
```sh
docker update [OPTIONS] CONTAINER [CONTAINER...]
```
5. Удалить все запущенные контейнеры:
```sh
docker kill $(docker ps -q)
```
6. Удалить все остановленные контейнеры:
```sh
docker rm $(docker ps -a -q)
```
7. Удалить все образы:
```sh
docker rmi $(docker images -q)
```
8. Почистить систему Docker:
```sh
docker system prune
docker system prune -af
```

На этом все.

### Дополнительные ссылки
1. [remove-docker-containers.md](https://gist.github.com/ngpestelos/4fc2e31e19f86b9cf10b)
2. [SO - How to clean up Docker](https://stackoverflow.com/questions/45798076/how-to-clean-up-docker)