---
layout: single
title:  "Mac os homebrew JAVA Install 홈브루 자바 설치"
categories: java
tag: java
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Homebrew JAVA Install 자바 설치  
<br />

adoptopenjdk/openjdk 추가하기  
```
brew tap adoptopenjdk/openjdk  
brew search jdk
```  
<br />

설치하기  
```
brew install —cask adoptopenjdk8
brew install —cask adoptopenjdk11
```  
<br />

자바 위치 확인  
```
/usr/libexec/java_home -V  
java —version
```  
<br />

사용 중인 shell 확인  
```
echo $SHELL
```  
<br />

환경변수 관리 zsh shell  
```
vi ~/.zshrc
```  
<br />

```
    # Java Paths
    export JAVA_HOME_8=$(/usr/libexec/java_home -v1.8)
    export JAVA_HOME_11=$(/usr/libexec/java_home -v11)

    # Java 8
    export JAVA_HOME=$JAVA_HOME_8

    # Java 11
    # export JAVA_HOME=$JAVA_HOME_11
```  
<br />

변경사항 적용 zsh shell  
```
source ~/.zshrc
```  
<br />