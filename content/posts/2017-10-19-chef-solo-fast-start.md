---
layout: post
title:  "Быстрый старт с Chef Solo"
date:   2017-10-19 09:00:00 +0800
categories: chef nix
---

***По мотивам [evtuhovich.ru](http://evtuhovich.ru/blog/2014/03/28/knife-solo/)***

0\. Предварительный этап

Убедитесь, что у вас установлен ruby-gem

1\. Для начала установим knife-solo:

```sh
gem install knife-solo
```

2\. Теперь создадим новую поваренную книгу в папке COOKBOOKNAME:

```sh
knife solo init COOKBOOKNAME
```

3\. Подготавливаем машину, которая будет получать наши поваренные книги (разумеется, проще будет, если вы уже скопировали туда свой ключ (ssh-copy-id ubuntu@myhostname)):

```sh
cd COOKBOOKNAME
knife solo prepare ubuntu@myhostname
```

После этого в папке COOKBOOKNAME\nodes появляется json-файл конфигурации машины, в котором нет ничего, кроме адреса (или имени) машины:

```json
{
  "run_list": [

  ],
  "automatic": {
    "ipaddress": "myhostname"
  }
}
```

4\. Начинаем пользоваться knife-solo, приведем json машины к следующему виду:

Создаем рецепт:
```sh
mkdir -p cookbooks/helloworld/recipes # создаем папку с именем рецепта
touch cookbooks/helloworld/recipes/default.rb # создаем пустой рецепт
```

В файл **default.rb** вставляем текст следующего содержания:

```rb
file "/tmp/helloworld.txt" do
  owner "root"
  group "root"
  mode 00544
  action :create
  content "Hello, Implementor!"
end
```

Здесь мы пишем рецепт на создание нового файлика в папке /tmp с указанным содержанием, кастомными правами и владельцем файла.

Приводим файл машины (nodes/myhostname.json) к следующему виду:

```json
{
  "run_list": [
        "recipe[helloworld]" # добавляем к запуску наш рецепт
  ],
  "automatic": {
    "ipaddress": "myhostname"
  }
}
```

5\. И варим:
```sh
knife solo cook myhostname
```

На этом все.

## Дополнительные ссылки

1. Разумеется, больше информации на [оф. сайте](https://docs.chef.io/chef_solo.html).
2. Небольшим подспорьем являлся репозиторий [macovsky](https://github.com/macovsky/rails-on-chef-solo).
3. Блог [alxschwarz](https://ru.alxschwarz.com/2012/04/chef-basics-introduction-to-cookbooks/).
4. Немного толковой информации на [Habrahabr](https://habrahabr.ru/company/epam_systems/blog/209368/).
