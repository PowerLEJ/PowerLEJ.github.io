---
layout: single
title:  "Mac M1 Github Blog Action"
categories: git
tag: git
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Mac M1 bundler jekyll Install  

맥에 루비가 깔려 있지만 ruby -v  
gem install bundler jekyll  
해도 깔리지 않는다  

```
brew install rbenv ruby-build
rbenv versions
rbenv install -l // 로 버전 확인하고
rbvens install 3.1.2 // 나는 3.1.2 버전을 깔았다

rbenv version // 를 확인하면 system에 *별표가 있다
rbvens global 3.1.2
rbenv version // 를 다시 확인하면 내가 설치한 버전에 *별표가 있다
```  

rbenv PATH 추가  
vi ~/.zshrc 에서 아래와 같이 추가한다  

```
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"
```  

이제 설치할 수 있다  

```
gem install bundler jekyll

jekyll -v
```  

## Github Blog Action

cd 명령으로 github.io 폴더로 이동
```
bundle exec jekyll serve
```
>
이동
```
    localhost:4000
```
