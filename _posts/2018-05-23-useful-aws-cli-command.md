---
layout: post
title:  "Полезные команды AWS CLI"
date:   2018-05-23 09:00:00 +0800
categories: nix 
---

1. Кому принадлежит образ AMI?
```
aws ec2 describe-images --profile PROFILE_NAME --filters Name=root-device-type,Values=ebs Name=architecture,Values=x86_64 --image-ids AMI_ID
```
2. Посмотреть список корзин в AWS
```
aws s3api list-buckets --profile=PROFILE_NAME --output text
```
3. Посмотреть группы безопасности EC2
```
aws ec2 describe-security-groups --profile=PROFILE_NAME --region REGION_NAME --output text
```
4. Получить список регионов AWS
```
aws ec2 describe-regions --profile=PROFILE_NAME --output text
```

На этом все.

### Дополнительные ссылки:
1. [7 Tips – Tuning Command Line History in Bash](https://www.shellhacks.com/tune-command-line-history-bash/)
