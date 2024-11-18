---
layout: single
title:  "Raspberry Pi LCD & 라즈베리파이 백업"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## LCD  
![20241118_01](/images/2024-11-18-Raspberry_class/20241118_01.jpg){: width="100%" height="100%"}{: .center}  
  
![20241118_02](/images/2024-11-18-Raspberry_class/20241118_02.jpg){: width="100%" height="100%"}{: .center}  
  
![Character_LCD_01](/images/2024-11-18-Raspberry_class/Character_LCD_01.PNG){: width="100%" height="100%"}{: .center}  
  
![Character_LCD_02](/images/2024-11-18-Raspberry_class/Character_LCD_02.PNG){: width="100%" height="100%"}{: .center}  
  
![Character_LCD_03](/images/2024-11-18-Raspberry_class/Character_LCD_03.PNG){: width="100%" height="100%"}{: .center}  

## 라즈베리파이 이미지 백업하기  
Win32 Disk Imager  
  
bootfs(D:)랑 SDHC(E:)가 나오는데 Win32 Disk Imager 에서  
Device는 boot인 [D:\]를 선택하지 않고, [E:\]를 선택하고 Read를 한다.  
Image File에 저장할 위치랑 저장하고 싶은 이름으로(image.img) 입력한다.  