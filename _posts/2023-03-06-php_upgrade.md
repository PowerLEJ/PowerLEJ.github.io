---
layout: single
title:  "Upgrade from PHP 7.x to PHP 8"
categories: php
tag: php
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Upgrade from PHP 7.x to PHP 8  

[출처](https://devanswers.co/how-to-upgrade-from-php-7-x-to-php-8-on-ubuntu-apache/){: .btn .btn--warning}  

```
dpkg -l | grep php | tee packages.txt

sudo apt-get purge php7.*
sudo apt-get autoclean
sudo apt-get autoremove

sudo add-apt-repository ppa:ondrej/php

sudo apt-get update

sudo apt-get install php8.1
sudo systemctl restart apache2

sudo apt install php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-intl -y
sudo apt install php8.1-fpm -y
sudo systemctl restart apache2

php -v

```  


