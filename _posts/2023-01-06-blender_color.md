---
layout: single
title:  "Blender 색칠"
categories: blender
tag: blender
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Blender 색칠  

1. 젤 위에 리본에서 Texture Paint 클릭  
2. 위에서 두 번째 리본 젤 왼쪽에 있는 셀렉트 박스에 Image Editor 클릭  
3. 그 바로 옆에 있는 셀렉트 박스에 Paint 클릭  
<br />
4. 3D 모형에서 Edit Mode로 가서 전체를 선택해서(A) 키보드 U를 눌러서 Mark Seam을 하고,  
5. Mode 2번으로 선으로 윗부분을 클릭한 다음 키보드 U 눌러서 Clear Seam 하고,  
6. 키보드 U 눌러서 Unwrap  
<br />
7. 두 번째 리본 젤 왼쪽에 있는 셀렉트 박스에 UV Editor 클릭 해서 보기 편하게 정리하고, 다시 Image Editor 클릭  
<br />
8. 오른쪽에 세로 리본 중 Material Properties(붉은 원 모양 아이콘) 에서 New로 이름 정해서 추가하고,  
9. Base Color를 Image Texture로 클릭하고 그 밑에 New로 이름 정하고 원하는 색깔 선택하고 OK  
10. 순수 색깔을 보고 싶을 때 위에서 두 번째 오른쪽에 있는 원 아이콘이 있는 젤 끝에 셀렉트 박스에서 Lighting을 Flat으로 선택  
<br />
11. 왼쪽 그림판에 위에서 두 번째 리번에서 중간 쯤에 +New 왼쪽에 셀렉트 박스를 클릭해서 아까 추가한 이름을 들고 올 수 있다  
<br />
12. 두 판 모두에서 색칠이 가능하다.  
<br />
13. 만약 오른쪽 판에 Texture Paint 색칠 안될 때 위에서 세 번째 리본의 젤 오른쪽에 Options > Backface Culling 체크 해제  