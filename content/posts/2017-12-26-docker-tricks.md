---
layout: post
title:  "Несколько трюков при работе с Docker + использование Portainer"
date:   2017-12-26 09:00:00 +0800
categories: docker cli
---


### Доступ в контейнер по ssh

Вообще, сама суть Docker теряется, когда начинаются изменения внутри самого контейнера, делать это крайне не рекомендуется. Однако, доступ иногда бывает необходим для дебага:

```
docker exec -it container_name /bin/bash
```

### Попробуем сделать из образа загружаемый сервис:

Берем [Portainer](https://github.com/portainer/portainer), который запускается вот так:

```
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /opt/portainer:/data portainer/portainer
```

Для создания сервиса используется команда:

```
docker service create
```

В исходной команде необходимо заменить параметр -v на --mount вида:

```
--mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock
```

В итоге получается:

```
docker service create -p 9000:9000 --mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock --mount type=bind,source=/opt/portainer,target=/data portainer/portainer
```

На этом все.

## Дополнительные ссылки

1. [Sysadmin.pm - Portainer](https://sysadmin.pm/portainer/)
2. [Github - Portainer](https://github.com/portainer/portainer)
