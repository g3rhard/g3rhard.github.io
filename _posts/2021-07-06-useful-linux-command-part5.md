---
layout: post
title:  "Полезные однострочники - часть 5"
date:   2021-07-06 09:00:00 +0800
categories: nix cli
---

*1*. Узнать ID приложения в MacOS

  ```sh
  APP_NAME=Slack
  osascript -e 'id of app "$APP_NAME"'
  ```

*2*. Удалить строчку из файла в MacOS

  ```sh
  sed -i '' '/pattern to match/d' FILE_NAME
  ```

*3*. Подмена заголовков, для тестирования локальных сайтов (не используя hosts файл):

  ```sh
  curl -H "Host: example.com" http://localhost/
  ```

*4*. Удалить дубликаты файлов, используя утилиту fdupes

  ```sh
  fdupes -r PATH -q -d --noprompt
  ```

*5*. Обновление версий Github Actions

  ```sh
  find . -type f -name "*.yml" | grep ".github/" | xargs sed -i '' 's/setup-helm@v1/setup-helm@v3/g
  ```

*6*. Красивый вывод состояния контейнеров Docker

  ```sh
  docker ps -a | perl -ne '@cols = split /\s{2,}/, $_; printf "%30s %20s %20s\n", $cols[1], $cols[3], $cols[4], $cols[6]'
  ```

На этом все.

### Дополнительные ссылки

1. [RIP Tutorial: Change HOST header](https://riptutorial.com/curl/example/31719/change-the--host---header)
