---
layout: post
title:  "Полезные однострочники - часть 1"
date:   2018-05-21 09:00:00 +0800
categories: nix cli
---

1. Кто занимает больше всего места в директории/на разделе?
```sh
du -s /FOLDER_NAME/*|sort -nr|cut -f 2-|while read a;do du -hs $a;done
```
2. Кто занимает больше всего места в оперативной памяти?
```sh
ps axo rss,comm,pid \
| awk '{ proc_list[$2]++; proc_list[$2 "," 1] += $1; } \
END { for (proc in proc_list) { printf("%d\t%s\n", \
proc_list[proc "," 1],proc); }}' | sort -n | tail -n 10 | sort -rn \
| awk '{$1/=1024;printf "%.0fMB\t",$1}{print $2}'
```
3. Кто занимает больше всего места в оперативной памяти и SWAP?
```sh
ps axo rss,comm,pid \
| awk '{ proc_list[$2] += $1; } END \
{ for (proc in proc_list) { printf("%d\t%s\n", proc_list[proc],proc); }}' \
| sort -n | tail -n 10 | sort -rn \
| awk '{$1/=1024;printf "%.0fMB\t",$1}{print $2}'
```
4. Распаковать архивы tar
```sh
tar xvf file.tar
tar xvzf file.tar.gz
tar xvzf file.tar.tgz
tar xvjf file.tar.bz2
tar xvjf file.tar.tbz2
```
5. Следить за изменением вывода
```sh
watch 'cat /proc/loadavg'
```
6. Наблюдение за содержимым файла
```sh
sudo tail -f /var/log/apache.log
```

На этом все.

### Дополнительные ссылки

1. [HowTo: Find Out Top Processes By Memory Usage In Linux](https://www.shellhacks.com/find-top-processes-memory-usage-linux/)
2. [HowTo: Extract Archives](https://www.shellhacks.com/extract-archive-tar-gz-bz2-rar-zip-7z-tbz2-tgz-z/)
3. [Полезные советы. Команда watch](http://nsk.lug.ru/poleznye-sovety/poleznye-sovety-komanda-watch/)
