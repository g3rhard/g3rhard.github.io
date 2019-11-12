---
layout: post
title:  "Полезные утилиты Mac OS X - Часть 1"
date:   2019-02-25 09:00:00 +0800
categories: mac brew
---

Если вы еще не используете [Brew](https://brew.sh/index_ru) - то самое время сделать это, ведь не смотря на огромный выбор приложений в AppStore - иногда бывает, что необходимые свободные утилиты с головой перекрывают потребности пользователей. Итак, вот мой список утилит, которыми я постоянно пользуюсь и которые доступны в Brew:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

1. [BitBar](https://getbitbar.com) - отличная утилита, не занимающая много памяти и позволяющая располагать оперативную информацию в верхней панели:
```sh
brew cask install bitbar
```
2. [Clipy](https://github.com/Clipy/Clipy) - менеджер буфера обмена, поддерживающий макросы:
```sh
brew cask install clipy
```
3. [Cyberduck](https://cyberduck.io) - Must have для всех, кто регулярно пользуется FTP, SFTP, AWS S3 и прочими облачными сервисами, отличный функционал, множество поддерживаемых фишек и протоколов:
```sh
brew cask install cyberduck
```
4. [iTerm2](https://www.iterm2.com) - лучший эмулятор терминала для Mac OS
```sh
brew cask install iterm2
```
5. [amethyst](https://ianyh.com/amethyst/) - оконный менеджер, основанный на коде xmonad, хорошо расставляет окна по экрану, хоть и версия пока что далека до стабильного релиза (0.12.2):
```sh
brew cask install amethyst
```
6. [mpsyt](https://github.com/mps-youtube/mps-youtube) - command line interface для тех, кто слушает музыку с Youtube. Для лучшей работы необходимо создать ключ Yotube API в https://console.developers.google.com/apis/api/youtube.googleapis.com/. Клиент прост, интуитивно понятен, есть возможность просмотра видео, используя [mpv](https://github.com/mpv-player/mpv).
7. [Dozer](https://github.com/Mortennn/Dozer) - программа для скрытия лишних значков из верхней панели Mac OS, что бы панель выглядела чистой.
```sh
brew cask install dozer
```
8. [TripMode](https://www.tripmode.ch/) - платная утилита ($), позволяющая ограничивать трафик для любых приложений, помогает в случае использования медленного интернета. Также умеет отключать соединение для приложений при переключении на VPN (правила гибко настраиваются).
```sh
brew cask install tripmode
```

И напоследок небольшой hack, как можно исправить, когда при выполнении brew upgrade получаем ошибку зависимостей:
```sh
/usr/bin/find "$(brew --prefix)/Caskroom/"*'/.metadata' -type f -name '*.rb' -print0 | /usr/bin/xargs -0 /usr/bin/perl -i -0pe 's/depends_on macos: \[.*?\]//gsm;s/depends_on macos: .*//g'
```

### Дополнительные ссылки

1. [Mac App Store - Search & Install any app on Mac](http://macappstore.org)
2. [homebrew - issue 58046](https://github.com/Homebrew/homebrew-cask/issues/58046)
3. [iTerm2 - window resize issue](https://superuser.com/questions/581889/iterm-2-window-resizing)
4. [iTerm2 - window resize issue 2](https://apple.stackexchange.com/questions/98342/changing-the-default-size-of-iterm2-when-it-opens/98406)
