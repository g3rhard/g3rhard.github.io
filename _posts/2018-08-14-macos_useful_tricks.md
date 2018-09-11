---
layout: post
title:  "Полезные однострочники для работы с Mac OS X"
date:   2018-08-14 09:00:00 +0800
categories: macos
---

1. Создать ссылку на папку iCloud в Finder для удобного доступа через cli:
```
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs iCloud
```
2. Безопасное удаление файлов с помощью rm (на самом деле не особенно)
```
rm -Pv FILE_NAME
rm -Pvrf ~/.FOLDER_NAME
```
3. Изменить количество иконок в Launchpad:
```
defaults write com.apple.dock springboard-columns -int N
defaults write com.apple.dock springboard-rows -int N
defaults write com.apple.dock ResetLaunchPad -bool TRUE;killall Dock
```
4. Поправить файл crontab:
```
sudo nano /var/at/tabs/ИМЯ_ПОЛЬЗОВАТЕЛЯ
```
5. Показывать/Не показывать скрытые файлы и папки
```
defaults write com.apple.finder AppleShowAllFiles -bool TRUE && killall Finder
defaults write com.apple.finder AppleShowAllFiles -bool FALSE && killall Finder
```

На этом все.

### Дополнительные ссылки:
1. https://apple.stackexchange.com/questions/252098/srm-gone-in-macos-sierra-10-12
2. https://github.com/herrbischoff/awesome-macos-command-line