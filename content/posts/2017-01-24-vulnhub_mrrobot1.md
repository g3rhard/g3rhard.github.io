---
layout: post
title:  "Разбор взлома VulnHub Mr. Robot: 1"
date:   2017-01-24 16:00:00 +0800
categories: hack
---

Недавно увидел статью на Хабре, где описывается прохождение CTF от VulnHub - Mr.Robot 1, стало интересно попробовать себя в роли Эллиота.

Основано на: https://habrahabr.ru/post/320106/ и https://jhalon.github.io/vulnhub-mr-robot1/

Для начала скачиваем последнюю версию тестовой машины (https://download.vulnhub.com/mrrobot/mrRobot.ova.torrent) и последнюю версию Kali Linux (https://www.kali.org/downloads).

### Первый флаг

Первый флаг находится перебором (https://github.com/GH0st3rs/robotscan)

{% highlight bash %}
./robotscan.py -u 'ip_machine' -e txt,php -w /usr/share/dirb/wordlists/big.txt -x 403
{% endhighlight %}

Либо просто просмотреть файл robots.txt на наличие файлов.

### Второй флаг

Вообще решений для второго флага было два: либо находится значение пары логин:пароль с помощью брутфорса patatorom и словаря fsocity.dic или внимательный просмотр файла /license.txt в котором находится зашифрованная base64 пара логин:пароль. IMHO, вариант один более труЪ, чем второй. Ну а увидеть в домашней папке ключ через прокинутый shell, расшифруя md5 пароля (password.raw-md5) не составит труда.
Прокинуть shell оказалось не так просто, нужно заккоментировать проверку наличия WordPress на атакуемой машине в коде эксплойта.

{% highlight ruby %}

 def exploit
    #fail_with(Failure::NotFound, 'The target does not appear to be using WordPress') unless wordpress_and_online?

{% endhighlight %}

После этого все заработало как надо.

### Третий флаг

Для поиска его нужно было найти доступные SUID приложения (цитата с хабра)

{% highlight bash %}
$ find / -perm -4000 2>/dev/null
{% endhighlight %}

В результате находится nmap уязвимой версии, который можно запустить с параметром --interactive, набираем !sh и проверяем свой id. Ну а ключ находится в папке /root, куда теперь мы имеем доступ.

voila!

На этом все.
