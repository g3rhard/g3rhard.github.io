---
layout: post
title:  "Полезные команды при работе с Docker"
date:   2018-09-11 09:00:00 +0800
categories: docker
---

1. Удалить неиспользуемые контейнеры (старые версии):
```
docker rmi $(docker images -q -f dangling=true)
docker rmi $(docker images | grep "^<none>" | awk '{print $3}')
```
2. Запустить shell внутри контейнера:
```
docker exec -it CONTAINER_NAME /bin/bash
```
3. Посмотреть список всех контейнеров:
```
docker ps --all
```
4. Обновить опции контейнера docker
```

```
5. Удалить все запущенные контейнеры:
```
docker kill $(docker ps -q)
```
6. Удалить все остановленные контейнеры:
```
docker rm $(docker ps -a -q)
```
7. Удалить все образы:
```
docker rmi $(docker images -q)
```
8. Почистить систему Docker:
```
docker system prune
docker system prune -af
```

На этом все.

### Дополнительные ссылки:
1. [remove-docker-containers.md](https://gist.github.com/ngpestelos/4fc2e31e19f86b9cf10b)
2. [SO - How to clean up Docker](https://stackoverflow.com/questions/45798076/how-to-clean-up-docker)