---
layout: post
title:  "MiniDLNA - Mac OS X"
date:   2018-08-22 09:00:00 +0800
categories: macos translate
---
### Черновик

Sony finally released a DLNA media player for the Playstation 4 on June 16, 2015. I don’t have a lot of streamable media, but what I do have is stored on my laptop.

I have used Plex in the past, but it is overly complicated and “heavy” for my needs. I wanted a light weight DLNA server that could be daemonized and point to a media directory. minidlna turned out to be the answer.

First, install Homebrew. You can install minidlna manually, but Homebrew makes everything much easier.

Once Homebrew is installed, install minidlna:

brew install minidlna
At the time of writing, I setup version 1.1.4.2 minidlna. The rest of this post will reference that version. Change the version as needed.

If you want minidlna’s directories, files, and logs to reside in the .config directory of your home folder, create the following directories. Otherwise, create them elsewhere.

```sh
mkdir -p /Users/foo/.config/minidlna
mkdir -p /Users/foo/.config/minidlna/media
mkdir -p /Users/foo/.config/minidlna/cache
```

For some reason brew does not setup the necessary minidlnad symlinks. So, create them manually:
```sh
cd /usr/local/bin
ln -s ../Cellar/minidlna/1.1.4_2/sbin/minidlnad minidlnad
```
Create file ~/.config/minidlna/minidlna.conf with the following contents. If your media is not stored in /Users/foo/.config/minidlna/media, change media_dir to the directory of your choice:

```
friendly_name=Mac DLNA Server
media_dir=/Users/foo/.config/minidlna/media
db_dir=/Users/foo/.config/minidlna/cache
log_dir=/Users/foo/.config/minidlna
```

Finally, start minidlnad:
```sh
minidlnad -f ~/.config/minidlna/minidlna.conf -P ~/.config/minidlna/minidlna.pid
```
If you have the OS X Firewall turned on, you will be prompted to allow minidlnad through the firewall. Of course, allow it through if you want to be able to stream anything.

By default, minidlnad will scan for new media every 895 seconds. You can change this by killing the current minidlnad process:
```sh
pkill minidlnad
```

And starting minidlnad with the following command:
```sh
minidlnad -t 60 -f ~/.config/minidlna/minidlna.conf -P ~/.config/minidlna/minidlna.pid
```
You can force a re-scan of your media directory by killing the minidlnad process and starting it with the following command:
```sh
minidlnad -R -f ~/.config/minidlna/minidlna.conf -P ~/.config/minidlna/minidlna.pid
```
Now, you should be able to open Media Player on your PS4 and stream your media. Supported media formats can be found here.

Небольшое дополнение: Стороннее ПО, которое предлагает трансляцию AirPlay на экран ПК, может мешать работе minidlna и подобного ПО, так как использует тот же порт (4500) для своих нужд.

На этом все.

## Дополнительные ссылки
1. [Install, Configure, and Use minidlna on OS X Yosemite](https://thornelabs.net/2015/08/23/install-configure-and-use-minidlna-on-os-x-yosemite.html)
