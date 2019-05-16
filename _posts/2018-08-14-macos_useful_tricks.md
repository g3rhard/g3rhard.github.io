---
layout: post
title:  "Полезные улучшения для работы с Mac OS X"
date:   2018-08-14 09:00:00 +0800
categories: macos cli
---

1. Создать ссылку на папку iCloud в Finder для удобного доступа через cli:
```sh
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs iCloud
```
2. Безопасное удаление файлов с помощью rm (на самом деле не особенно):
```sh
rm -Pv FILE_NAME
rm -Pvrf ~/.FOLDER_NAME
```
3. Изменить количество иконок в Launchpad:
```sh
defaults write com.apple.dock springboard-columns -int N
defaults write com.apple.dock springboard-rows -int N
defaults write com.apple.dock ResetLaunchPad -bool TRUE;killall Dock
```
4. Поправить файл crontab:
```sh
sudo nano /var/at/tabs/ИМЯ_ПОЛЬЗОВАТЕЛЯ
```
5. Показывать/Не показывать скрытые файлы и папки:
```sh
defaults write com.apple.finder AppleShowAllFiles -bool TRUE && killall Finder
defaults write com.apple.finder AppleShowAllFiles -bool FALSE && killall Finder
```
6. Показывать только открытые приложения в Dock:
```sh
defaults write com.apple.dock static-only -bool TRUE
killall Dock
```
7. Использовать plain text в TextEdit по умолчанию:
```sh
defaults write com.apple.TextEdit RichText -int 0
```
8. Не записывать файлы .DS_Store на сетевые диски и USB:
```
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true
```

На этом все.

### Дополнительные ссылки
1. [StackExchange - SRM gone in macOS Sierra (10.12)](https://apple.stackexchange.com/questions/252098/srm-gone-in-macos-sierra-10-12)
2. [herrbischoff/awesome-macos-command-line](https://github.com/herrbischoff/awesome-macos-command-line)
3. [iMore - 15 Terminal commands that every Mac user should know](https://www.imore.com/fifteen-terminal-tricks-every-mac-user-should-know)
