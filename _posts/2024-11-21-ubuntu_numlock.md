---
layout: single
title:  "Ubuntu20.04 Num Lock 항상 켜기"
categories: linux
tag: linux
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)
  
## 우분투 20.04에서 Num Lock 항상 켜기  
```
sudo apt install numlockx
```  

```
sudo vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
```  

50-ubuntu.conf 파일 안에 greeter-setup-script = / usr / bin / numlockx on 추가하기  

```
[Seat:*]
user-session=ubuntu
greeter-setup-script = / usr / bin / numlockx on
```  