---
layout: post
title:  "Let's Encrypt: Route53 + OrangePi (ARM)"
date:   2022-07-22 09:00:00 +0800
categories: cli docker aws route53
---

## TLDR

  ```sh
  docker run -it --rm --name certbot \
  --env AWS_ACCESS_KEY_ID=access_key \
  --env AWS_SECRET_ACCESS_KEY=secret_key \
  -v "$(pwd)/etc-letsencrypt:/etc/letsencrypt" \
  -v "$(pwd)/var-letsencrypt:/var/lib/letsencrypt" \
  certbot/dns-route53:arm32v6-nightly certonly \
  -d DOMAIN.COM \
  -d '*.DOMAIN.COM' \
  -m 'MAIL@EXAMPLE.COM' \
  --agree-tos --non-interactive \
  --dns-route53 \
  --server https://acme-v02.api.letsencrypt.org/directory
  ```

## Points

* certbot/dns-route53:arm32v6-nightly - image for OrangePi
* --non-interactive - silent mode without interaction
* --dns-route53 - using module for AWS Route53

That's all.

## Additional links

1. [Wai Loon - Generate Standalone SSL Certificate with Let’s Encrypt for AWS Route 53 using Docker](https://medium.com/w-logs/generate-standalone-ssl-certificate-with-lets-encrypt-for-aws-route-53-25a30ca3062)
2. [Automating Certificates with Certbot in Docker](https://coderevolve.com/certbot-in-docker/)
3. [Let's Encrypt Wildcard Certificate Configuration with AWS Route 53](https://medium.com/prog-code/lets-encrypt-wildcard-certificate-configuration-with-aws-route-53-9c15adb936a7
)
