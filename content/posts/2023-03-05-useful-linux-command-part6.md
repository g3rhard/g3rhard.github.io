---
layout: post
title:  "Полезные однострочники - часть 6"
date:   2023-03-05 09:00:00 +0800
categories: nix cli
---

*1*. Остановить приложение по имени и запустить снова в MacOS (работает для Amethyst, который очень часто вылетает в последних релизах в многомониторной конфигурации)

```sh
osascript -e 'quit app "Amethyst"'; open -a "Amethyst"
```

*2*. Конвертировать JFIF файлы в каталоге в PNG (используется convert из пакета ImageMagick)

```sh
for i in *.jfif; do convert "$i" "${i%.*}.png"; done
```

*3.* Установить/Обновить все плагины в Vim (для установленного Vundle)

```sh
vim +PluginInstall +qall
vim +PluginUpdate +qall
```

*4.* Удалить пустые и закомментированные строки в файле

```sh
sed '/^[[:blank:]]*#/d;s/#.*//' FILENAME
```

*5.* Проверить ratelimits для API (например, для fail2ban)

```sh
for i in {0..20}; do (curl -Is --request GET -H "Content-Type: application/json" "API_URL"  | head -n1 >> check_rate_limit.log &) 2>/dev/null; done
```

На этом все.

## Дополнительные ссылки

1. []()
