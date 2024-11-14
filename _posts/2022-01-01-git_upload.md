---
layout: single
title:  "Github Upload"
categories: git
tag: git
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Github Upload

```
git init # 현재 디렉토리를 기준으로 Git 저장소가 생성
git status # 파일 상태 확인
git remote -v # 현재 연결된 레포 확인
git remote add origin 깃허브주소 # 깃허브와 연결

git add . # 현재 디렉토리의 모든 변경 내용을 스테이징 영역에 올림
git commit -m "메시지" # 변경 작업을 로컬 저장소에 기록
git push --set-upstream origin master # 연결된 깃허브에 push
```
