---
layout: post
title:  "How I switched to Hugo for blog"
date:   2023-01-30 09:00:00 +0800
categories: blog
---

После некоторых исследований, пришел к выводу, что [Jekyll](https://jekyllrb.com/docs/) и его темы, хоть и выглядят доступными, однако не так часто обновляются и требуют установки разных зависимостей (например Ruby) на систему, в которой будет писаться блог, для отладки стиля и прочего. Из последних проблем - давно не обновляемые зависимости для работы с SCSS, которые при запуске выдают много разных Warning, из-за функций, который скоро будут deprecated.

Из требований к системе было:

* Поддержка GitHub Pages
* Поддержка Markdown
* Простая конфигурация
* Разные модули для поддержи shortcodes (например для GitHub Gist)
* Легкая миграция существующего блога на новую платформу
* Поддержка разных социальных кнопок
* Поддержка RSS

Ко всему этому внезапно подошел [Hugo](https://gohugo.io/about/), как говорится в описании "Самый быстрый фреймворк для построения веб-сайтов", из положительных моментов:

* Экстремально быстрая сборка страниц (< 1ms)
* Кроссплатформенность
* "Живая" перезагрузка страниц

## Миграция существующего блога

Без долгих слов, давайте приступим к делу:

```sh
brew install hugo # установка для MacOS
hugo new site quickstart # создаем новый каталог quickstart с начальными файлами внутри
cd quickstart
git init # инициализируем git
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod # добавляем субмодуль git для темы
echo 'theme: "PaperMod"' >> config.yaml # добавляем тему в конфиг (о нем чуть позже)
hugo server # запускаем сервер
```

Копируем записи со старого блога:

```sh
mkdir -p content/posts
cp -R ../g3rhard.github.io/_posts/* ../quickstart/content/posts
```

Проверяем, что блог работает, открыв страницу [http://localhost:1313](http://localhost:1313)

## Дополнительные улучшения

Вероятно, не все работает, как ожидается, поэтому по документации к теме добавляем и другие параметры:

```yaml
baseURL: "https://g3rhard.github.io/"
languageCode: "en-us"
title: "G3rhard"
paginate: 5
theme: "PaperMod"
rssLimit: 10

googleAnalytics: "UA-125136651-1"

minify:
  disableXML: true
  minifyOutput: true

outputs:
  home:
    - HTML
    - RSS
    - JSON

params:
  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  title: G3rhard blog
  description: "System Engineer from Gdansk"
  keywords: [Blog, Portfolio, PaperMod]
  author: Me
  # author: ["Me", "You"] # multiple authors
  # images: ["<link or path of image for opengraph, twitter-cards>"]
  DateFormat: "January 2, 2006"
  defaultTheme: auto # dark, light
  disableThemeToggle: false

  ShowReadingTime: true
  ShowShareButtons: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowCodeCopyButtons: false
  ShowWordCount: true
  ShowRssButtonInSectionTermList: true
  UseHugoToc: true
  disableSpecial1stPost: false
  disableScrollToTop: false
  comments: false
  hidemeta: false
  hideSummary: false
  showtoc: true
  tocopen: false

  homeInfoParams:
    Title: "Hi there \U0001F44B"
    Content: Welcome to my blog

  socialIcons:
    - name: github
      url: "https://github.com/g3rhard"
    - name: buymeacoffee
      url: "https://buymeacoffee.com/g3rhard"
    - name: linkedin
      url: "https://linkedin.com/g3rhard"
    - name: Rss
      url: "index.xml"

markup:
  highlight:
    noClasses: false
    # anchorLineNos: true
    # codeFences: true
    # guessSyntax: true
    # lineNos: true
    # style: monokai
```

Копируем файлы favicon в каталог static и добавляем конфигурацию в config.yml:

```sh
❯ tree static
static
└── img
   ├── apple-touch-icon.png
   ├── favicon-16x16.png
   ├── favicon-32x32.png
   ├── favicon.ico
   └── safari-pinned-tab.svg
```

```yaml
params:
  assets:
    favicon: "/favicon.ico"
    favicon16x16:  "/favicon-16x16.png"
    favicon32x32:  "/favicon-32x32.png"
    apple_touch_icon:  "/apple-touch-icon.png"
    safari_pinned_tab:  "/safari-pinned-tab.svg"
```

И последний штрих - добавляем .gitignore:

```sh
# Generated files by hugo
/public/
/resources/_gen/
/assets/jsconfig.json
hugo_stats.json

# Executable may be added to repository
hugo.exe
hugo.darwin
hugo.linux

# Temporary lock file while building
/.hugo_build.lock
```

That's all.

## Additional links

1. [Hugo](https://gohugo.io/about/)
2. [github - adityatelange/hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod)
3. [Favicon Generator](https://realfavicongenerator.net)
4. [favicon.io](https://favicon.io)
