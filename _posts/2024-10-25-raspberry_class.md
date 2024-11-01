---
layout: single
title:  "Raspberry Pi 4"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## VNC 사용하기  

라즈베리파이4에서  
```
sudo raspi-config
```  

3 Interfaace Options 클릭  
I3 VNC 들어가서 no 되어 있는 것을 yes 로 설정한다.  
  
윈도우에서 vnc viewer 다운로드 받는다.  

## 라즈베리파이4와 노트북 이더넷 유선으로 연결하기

라즈베리파이4 터미널에서  
```
sudo ifconfig eth0 inet 192.168.101.101 netmask 255.255.255.0
```  
근데 리부팅 하면 지워진다.  

## 윈도우에서  
![20241025_001](/images/2024-10-25-class/20241025_001.PNG){: width="100%" height="100%"}{: .center}  
네트워크 및 인터넷 설정 열기  
오른쪽 마우스  
어댑터 옵션 변경  
이더넷(Realtek PCle GbE Family Control...) > 인터넷 프로토콜 버전 4(TCP/IPv4) >  
IP 주소 : 192.168.101.102, 서브넷 마스크 : 255.255.255.0  

## 라즈베리파이 GUI에서 고정IP 설정하기  
![20241025_001](/images/2024-10-25-class/20241025_002.PNG){: width="100%" height="100%"}{: .center}  
Advanced Options > Edit Connections... > IPv4 Settings 에서 Address : 192.168.101.101 하고 Save  

## 윈도우의 VSCode 에 라즈베리파이 서버와 SSH 연결  
확장:마켓플레이스에서 ssh 검색 후 설치 (Remote - SSH 를 설치했음)  
설치하면 왼쪽 바에 마켓플레이스 밑에 원격탐색기 아이콘이 나옴  
원격/(터널/SSH) > SSH 에 +(새원격) 클릭 해서 사용자명@192.168.101.101 등등 하라는대로 하면 됨  

## 포트 확인  
```
netstat -tnlp
```  
구글에 well known port 라고 검색하면 잘 알려진 포트를 볼 수 있다.  