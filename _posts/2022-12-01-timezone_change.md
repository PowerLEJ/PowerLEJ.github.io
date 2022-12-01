---
layout: single
title:  "Linux Ubuntu Timezone Change"
categories: linux
tag: linux
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Linux Ubuntu Timezone Change  

1. 현재 일시 확인  
```
date
```  

2. /usr/share/zoneinfo/ 에서 원하는 국가 확인  
```
cd /usr/share/zoneinfo/
ls
```  

3. 원하는 국가 설정  

예시  

```
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```  

```
sudo ln -sf /usr/share/zoneinfo/Turkey /etc/localtime
```  
