---
layout: single
title:  "Linux apt Package Check"
categories: linux
tag: linux
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)
  
## 리눅스 apt 패키지  
### 설치 가능한 패키지 검색  
```
apt list 패키지명
```  

### 설치된 패키지 검색  
```
apt --installed list
```  

```
apt --installed list “*turtle*”
```  

### 설치된 패키지 삭제  
```
apt remove 패키지명
```  

### 도움 받기  
```
apt help
```  