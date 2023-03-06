---
layout: single
title:  "Blender 입체 캐릭터"
categories: blender
tag: blender
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Blender 입체 캐릭터  

### Subdivision Surface  
정육면체에서 Subdivision Surface > Levels Viewport 설정 > Subdivision 옆에 4개 아이콘(모니터, 카메라 등) 옆에 셀렉트 박스 클릭 후 Apply 클릭(Ctrl + a)  
Apply 안하면 Edit Mode에서 정육면체 상태로 All 선택되어 있음.  

### Mirror  
1. Edit Mode에서 1번 점 선택 모드에서 Shift+x로 투시모드로 절반을 x키 누르고 Faces 클릭.
2. Object Mode로 가서 오른쪽 세로 리본 Modifier Properties(니퍼아이콘) > Add Modifier > Mirror > Mirror 옆에 4개 아이콘(모니터, 카메라 등)에서 첫 번째에 있는 꼭지점이네모인삼각형아이콘 클릭, Clipping 체크하기  

### 다리 뚫기  
1. Edit Mode에서 1번 점 선택 모드에서 점 선택 후 x키 눌러서 Vertices 클릭  
2. 뚫린 부분 중 점 하나 클릭 후 Alt 누르고 바로 옆에 점도 같이 눌러서 다 선택한 다음  
3. Proportional Editing Proportional edit mode(원안에색칠된작은원아이콘) 해제된 상태에서  
4. 위에서 두 번째 리본의 Mesh 클릭 > Transform > To Sphere  

### 다리 늘리기  
1. Edit Mode에서 1번 점 선택 모드에서 뚫린 부분 중 점 하나 클릭 후 Alt 누르고 바로 옆에 점도 같이 눌러서 다 선택한 다음  
2. e키로 늘린다.  
3. s키를 누르고, z키를 누르고, 넘버0키를 누른다. 
4. 밑에서 봤을 때 마감을 해줘야 하는데 e키 누르고 s키 눌러서 안으로 들여보내준다. m키 눌러서 At Center 클릭

### 뭔가를 붙이기  
1. Edit Mode에서 3번 면 선택 모드에서 얼굴의 일부분을 Shift + d 복붙하고, p키 클릭 후 Selection 클릭  
2. Object Mode로 가서 오른쪽 세로 리본 Modifier Properties(니퍼아이콘) > Add Modifier >Solidify 클릭  
3. Thickness로 굵기 조절, Subdivision 옆에 4개 아이콘(모니터, 카메라 등) 옆에 셀렉트 박스 클릭 후 Apply 클릭(Ctrl + a)  
4. Subdivision에서 Levels Viewport, Render 조절  

### 수염처럼 나오게, 눈뼈처럼 들어가게  
1. Edit Mode에서 3번 면 선택 모드에서 면 선택 후 Alt + e 누르고, Extrude Faces Along Normals 클릭 후 안이나 밖으로 조절  