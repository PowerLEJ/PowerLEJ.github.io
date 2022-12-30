---
layout: single
title:  "Blender Rigging"
categories: blender
tag: blender
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Blender Rigging  

1. Shift + a > Armature  
2. 오른쪽 세로 리본에 Object Properties(주황네모아이콘)에서 Viewport Display 펼쳐서 Show에 In Front 체크하기    
<br />
3. 중요: Edit Mode를 나가면 안된다.  
Edit Mode로 이동하고 E로 추가한다.  
E로 추가하는 것 말고 다른 위치에 또 다른 뼈를 두고 싶으면 원하는 위치에 Shift+오른쪽마우스 클릭 후 Shift+a 를 누른다.  
4. 몸통뼈와 팔뼈를 잇기 위해 부모 자식 관계를 형성해야 한다. 자식인 팔뼈를 클릭 하고 Shift + 부모인 몸통뼈를 클릭한다. 자식인 팔뼈는 주황색, 부모인 몸똥뼈는 노란색이 된다.  
5. Ctrl + p 를 누르고, Keep Offset 클릭.  
6. 반대편은 미러링 하는데 Shift + d 눌러서 복붙하고 오른쪽마우스 클릭해서 Mirror > X Global 클릭.    
<br />
7. Pose Mode로 가서 뼈와 살을 붙인다. 제일 상단 리본 Edit > Lock Object Modes 체크 해제  
8. 먼저 살(오브젝트)을 클릭한 다음에 뼈대 클릭  
9. Ctrl + p > Bone 클릭  
10. 키보드 r로 방향 돌리다가 원래 위치로 복원하고 싶을 때는 Alt + r 누른다.  
11. 애니메이션 저장을 위해서는 키보드 a로 전체 선택 후, 키보드 i 누르고, Location, Rotation & Scale 클릭.  
<br />
쉬운 캐릭터의 경우  
4-1. Object Mode에서 뼈와 살을 선택한 후 Ctrl + p를 누르고 With Automatic Weights 클릭.  
4-2. 부모 자식 관계가 제대로 형성되지 않았을 때는 Pose Mode에서 자식들을 선택하고, Edit Mode로 가서 부모를 선택하고, Ctrl + p를 누르고 Keep Offset 클릭.  