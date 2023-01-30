---
layout: post
title:  "Установка Android 7.1 на Raspberry Pi 3"
date:   2017-10-16 09:00:00 +0800
categories: android
---

1. Установка Android на sd карту:

	a. Загрузите дистрибутив Android [aosp-7.1.2_r17-rpi3-20170628.tar.gz][AOSP]
	b. Запишите его на SD с помощью [Etcher.io](https://etcher.io)

2. Для того что бы была возможность использовать ADB установите себе ADB:

	a. *buntu/deb
```sh
	apt install -y adb
```
	b. Включите функцию отладки по USB в "Параметрах разработчиков" на устройстве.

3. После этого нужно всего два шага, что бы устанавливать .apk
```sh
	adb connect IP_ADDRESS # Подключаемся к устройству
	adb install APP.apk # Используя подключение устанавливаем *.apk
```

4. Для установки Google Apps загрузите архив [open_gapps-arm-7.1-tvstock-20170420-rpi3.tar.gz][GApps]:
```sh
	tar zxvf open_gapps-arm-7.1-tvstock-20170420-rpi3.tar.gz
	./gapps.sh -a IP_ADDRESS
```

Надеюсь обновление следует, поскольку по дефолту заметны внезапные артефакты изображения (скорее всего связаны с параметрами в config.txt)

Также стоит обратить внимание на то, что по умолчанию выставлено разрешение в 1280х720 в файле config.txt.

## Дополнительные ссылки

1. [GApps](https://files.ime.moe/Android/Raspberry.Pi.3/open_gapps-arm-7.1-tvstock-20170420-rpi3.tar.gz)
2. [AOSP](https://files.ime.moe/Android/Raspberry.Pi.3/aosp-7.1.2_r17-rpi3-20170628.tar.gz)
