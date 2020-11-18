---
layout: post
title:  "Использование ffmpeg для работы с видео"
date:   2019-02-16 09:00:00 +0800
categories: ffmpeg video cli
---

1. Вырезаем нужный фрагмент из видео:
```sh
ffmpeg -ss HH:MM:SS -t HH:MM:SS -i VIDEO.EXT -vcodec copy -acodec copy VIDEO.cut.EXT
-ss Start time
-t Duration
```
2. Понижение качества всех mp3 файлов до 128kb/s:
```sh
find . -iname "*.mp3" -type f -exec ffmpeg -i {} -codec:a libmp3lame -qscale:a 5 {.mp3,.128.mp3} -y \; -exec /bin/rm {} \;
```

### Дополнительные ссылки
1. [Вырезать фрагмент из видео. FFmpeg](http://www.kompx.com/ru/vyrezat-fragment-iz-video-ffmpeg.htm)
2. [Github - Avidemux](https://github.com/mean00/avidemux2)
