---
layout: post
title:  "Расширение функционала Shell"
date:   2019-03-01 09:00:00 +0800
categories: shell nix
---

Один из моментов, как можно ускорить работу в shell, помимо использования более быстрого/удобного shell (zsh,tcsh,ksh,fish, etc) - это использование алиасов в работе, в частности, рассмотренные примеры будут касаться bash/zsh.

Некоторые алиасы уже идут в плагинах zsh, некоторые же можно (и иногда нужно) придумывать самому.

```sh
# Сокращаем написание команд
alias ls='ls -lh'
# Сокращаем написание директорий
alias test_dir='cd ~/test_dir'
# Запускаем VLC из Terminal
alias vlc='/Applications/VLC.app/Contents/MacOS/VLC'
# И запускаем радио
alias chillout='vlc http://air.radiorecord.ru:805/chil_64 &'
```

*TODO: Запись еще не закончена*

### Дополнительные ссылки

1. [30 Handy Bash Shell Aliases For Linux / Unix / Mac OS X](https://www.cyberciti.biz/tips/bash-aliases-mac-centos-linux-unix.html)
2. [Digital Ocean - An Introduction to Useful Bash Aliases and Functions](https://www.digitalocean.com/community/tutorials/an-introduction-to-useful-bash-aliases-and-functions)
3. [Gist - ioparaskev](https://gist.github.com/ioparaskev/88875c83bcab519f017b9cdd0b1960e7)
4. [xakep.ru - Прокачай терминал! Полезные трюки, которые сделают тебя гуру консоли](https://xakep.ru/2017/05/18/cli-console-tips/)
5. [How to run VLC command in mac terminal](https://superuser.com/questions/997673/how-to-run-vlc-command-in-mac-terminal)
