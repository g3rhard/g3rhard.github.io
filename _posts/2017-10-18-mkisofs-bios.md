---
layout: post
title:  "Создание загрузочного образа .iso с DOS для обновления BIOS"
date:   2017-10-18 09:00:00 +0800
categories: new hack 
---

Иногда возникает необходимость обновления BIOS на материнских платах. Некоторые производители добавляют пункт обновления BIOS прямо в меню BIOS, некоторые требуют только ручного обновления из DOS.

- Устанавливаем утилиту mkisofs:

a. buntu:
```sh
apt install -y genisoimage
```
b. macos:
```sh
brew install dvdrtools
```
- Скачиваем и создаем загрузочный образ:
```sh
mkdir boot_dos_iso #
cd boot_dos_iso/
wget -N http://www.fdos.info/bootdisks/ISO/FDOEMCD.builder.zip
unzip FDOEMCD.builder.zip
cd FDOEMCD
mkisofs -o testdisk.iso -V "DOS CD" \
-b isolinux/isolinux.bin -no-emul-boot \
-boot-load-size 4 -boot-info-table -N -J -r -c boot.catalog \
-hide boot.catalog -hide-joliet boot.catalog CDROOT
```

- Добавить нужные файлы (образы BIOS, утилиты прошивки можно, добавив их в FDOEMCD\CDROOT)

P.S. Небольшой нюанс, когда собирается образ для прошивки серверов Supermicro, необходимо переименовать файлы для успешной прошивки:

```sh
cd CDROOT\
cp AFUDOSU.{smc,exe}
cp AFUDOSU.EXE AFUDOS.EXE 
cp CHOICE.{smc,exe}
cp FDT.{smc,exe}
```

На этом все.