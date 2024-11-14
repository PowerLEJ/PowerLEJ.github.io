---
layout: single
title:  "Window10 Date Settings"
categories: window
tag: window
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 윈도우10 작업표시줄 시계 초단위 표시 설정  
윈도우 + R > regiedit 입력하여 레지스트리 편집기 열기  

```
컴퓨터\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced
```  

![window10_date_settings_01](/images/2024-11-12-window_date_settings/window10_date_settings_01.PNG){: width="100%" height="100%"}{: .center}  

작업 관리자 > 프로세스 > 앱 > Windows 탐색기 다시시작  

## 윈도우10 작업표시줄 날짜 요일 표시 설정  
1. 작업표시줄 우측 하단 시계 쪽에 오른쪽 마우스 > 날짜/시간 조정(A)  
2. 스크롤 좀 내려서  
3. 관련 설정 > 다른 시간대에 대한 시계 추가 >  
4. 날짜 및 시간 > 날짜 및 시간 변경(D)... >  
5. 달력 설정 변경 > (날짜 및 시간 설정)  
6. 추가 설정(D)... > (국가 또는 지역)  
7. 날짜 > 날짜 형식 > 간단한 날짜(S): yyyy-MM-dd '('ddd')' > 확인  

![window10_date_settings_02](/images/2024-11-12-window_date_settings/window10_date_settings_02.PNG){: width="100%" height="100%"}{: .center}  