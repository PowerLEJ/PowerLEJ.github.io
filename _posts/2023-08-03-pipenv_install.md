---
layout: single
title:  "Mac os M1 pipenv Install 설치"
categories: pipenv
tag: pipenv
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## pipenv Install 설치  

### brew로 pipenv 설치  
```
brew install pipenv
```  
<br />
mkdir 로 폴더 만든 후  

### 가상환경 생성  
```
pipenv —python 3.9 
```  

### 가상환경 제거  
```
pipenv —rm 
```  

### 가상환경에서 실행만 하고 가상환경 활성화는 하지 않음  
```
pipenv run COMMANDS
ex) pipenv run python
```  

### 가상환경 실행  
```
pipenv shell
```  

### 패키지 설치  
```
pipenv install pandas
pipenv install jupyter
pipenv install ipykernel
```  

### 정확한 위치 확인  
```
pipenv —venv
pipenv —py
```  