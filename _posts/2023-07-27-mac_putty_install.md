---
layout: single
title:  "Mac PuTTY Install"
categories: mac
tag: mac
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Mac PuTTY Install  

```
brew instsall putty
```  

[macport 설치 Click](https://www.macports.org/install.php){: .btn .btn--warning}  

나는 macOS Ventura v13 설치함  

```
sudo port -v selfupdate
sudo port install putty // continue? Y 누름
```  

이런 게 뜨면  

```
python311 has the following notes:
    To make this the default Python or Python 3 (i.e., the version run by the
    'python' or 'python3' commands), run one or both of:

        sudo port select --set python python311	<-
        sudo port select --set python3 python311 <-
```  

```
sudo port select --set python python311
sudo port select --set python3 python311
```  

터미널에 putty 입력  
```
putty
```  

오류 뜨면  

[XQuartz 설치 Click](https://www.xquartz.org/){: .btn .btn--warning}  

설치 완료되면 로그아웃 됨  
다시 로그인 하고  
터미널에 putty 입력  
```
putty
```  

### pem 파일을 ppk 파일로 변환  
```
puttygen 파일명.pem -o 파일명.ppk
```  