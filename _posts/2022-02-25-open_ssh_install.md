---
layout: single
title:  "Open SSH Install"
categories: ssh
tag: ssh
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Open SSH Install  
<br />

Open SSH Install  
```
sudo apt-get install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```  

linux terminal ssh connect  
```
ssh user@server-name // (ex. ssh pi@192.168.219.112)
```  