---
layout: single
title:  "Github Upload"
categories: memo
tag: memo
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Github Upload

```
git init
git status
git add .
git commit -m "메시지"
git remote add origin 깃허브주소
git push --set-upstream origin master
```
