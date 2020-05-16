---
layout: post
title:  "Полезные однострочники - часть 4"
date:   2020-05-06 09:00:00 +0800
categories: nix cli
---

1. Когда нужно разделить развороты внутри файла PDF:

```sh
# Разделить по вертикали
mutool poster -x 2 source.pdf out.pdf
# Разделить по горизонтали
mutool poster -y 2 source.pdf out.pdf
```

На этом все.

### Дополнительные ссылки

1. [LOR - 12967886](https://www.linux.org.ru/forum/talks/12967886)
