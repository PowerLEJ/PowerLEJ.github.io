---
layout: single
title:  "Git Multiple Accounts SSH Setting 깃 여러 개 계정 SSH 설정"
categories: git
tag: git
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

### SSH Key 생성  

SSH 확인하기  
```
cd ~/.ssh
ls -al
```  

```
ssh-keygen -t rsa -C "유저1이메일" -f "유저1이름"
```  

암호는 입력하기 귀찮아서 Enter  

```
ssh-keygen -t rsa -C "유저2이메일" -f "유저2이름"
```  

확인하기  
```
ls -al ~/.ssh
```  

```
ssh-add ~/.ssh/유저1이름
ssh-add ~/.ssh/유저2이름
```  

확인하기
```
ssh-add -l
```  

삭제하는 법
```
ssh-add -D
```  

### 유저1이름과 유저2이름이 각각 할 일  

SSH 공개키 추가하기  
```
vi ~/.ssh/유저1이름.pub
```  

복사해서 GitHub > Setting 메뉴 > SSH and GPS keys > New SSH key 에 붙여 넣기  

### config 파일  

vi ~/.ssh/config 를 치고, config 파일이 없으면 만들고, 아래 내용을 입력한다.  

```
#유저1이름의 SSH 설정
Host github.com-유저1이름
    HostName github.com
    User 유저1이름
    IdentityFile ~/.ssh/유저1이름

#유저2이름의 SSH 설정
Host github.com-유저2이름
    HostName github.com
    User 유저2이름
    IdentityFile ~/.ssh/유저2이름  
```  

SSH 연결 잘 되었나 확인  
```
ssh -T git@github.com-유저1이름
```  

###  GitHub Clone 방법  
GitHub에서 clone 받을 때 github.com 뒤에 -유저1이름 넣기  
```
git clone git@github.com-유저1이름:유저1이름/파일명.git
```  

### Git Local User 추가  
```
git config -local user.name "깃이름"
git config -local user.email "깃이메일"
```  