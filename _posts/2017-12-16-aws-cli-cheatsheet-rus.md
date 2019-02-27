---
layout: post
title:  "AWS CLI Cheatsheet (перевод)"
date:   2017-12-16 09:00:00 +0800
categories: aws translate cli
---

1. Получить список ключей EC2
```
# http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-key-pairs.html
aws ec2 describe-key-pairs
```
2. Импортировать свой ключ
```
aws ec2 import-key-pair \
    --key-name keyname_test \
    --public-key-material file:///home/apollo/id_rsa.pub
```
3. Получить список групп IAM
```
aws iam list-groups
```
4. Получить список ключей доступа IAM
```
# list all access keys
aws iam list-access-keys
```
5. Получить время последнего доступа по ключу IAM
```
aws iam get-access-key-last-used --access-key-id AKIAINA6AJZY4EXAMPLE
```
6. Деактивировать ключ IAM
```
aws iam update-access-key \
    --access-key-id AKIAI44QH8DHBEXAMPLE \
    --status Inactive \
    --user-name aws-admin2
```
7. Получить список пользователей IAM
```
aws iam list-users --no-paginate
```

***По мотивам [AWS CLI Cheatsheet](https://gist.github.com/apolloclark/b3f60c1f68aa972d324b).***

На этом все.

### Дополнительные ссылки:
1. [AWS CLI Cheat Sheet](https://github.com/toddm92/aws/wiki/AWS-CLI-Cheat-Sheet) by [toddm92](https://github.com/toddm92)
