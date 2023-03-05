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

На этом все.

## Дополнительные ссылки

1. []()
