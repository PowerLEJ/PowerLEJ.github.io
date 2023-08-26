---
layout: single
title:  "SQL Backup"
categories: sql
tag: sql
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

### 프로시저 백업하기  

```
mysqldump --routines --no-create-info --no-data --no-create-db --skip-opt -u 아이디 -p 데이터베이스명 > 백업받을파일명

ex) mysqldump --routines --no-create-info --no-data --no-create-db --skip-opt -u root -p schema_name > file_name.sql
```  

### 테이블 스키마만 백업하기  
```
mysqldump -u 아이디 -p --no-data 데이터베이스명 > 백업받을파일명

ex) mysqldump -u root -p --no-data schema_name > file_name.sql
```  

### 테이블 백업하기 (데이터 포함)  
```
mysqldump -u 아이디 -p 데이터베이스명 테이블명 > 파일명

ex) mysqldump -u root -p schema_name table_name > file_name.sql
```  

### 데이터 넣기  
```
mysql -u 아이디 -p 데이터베이스명 < 파일명.sql

ex ) mysql -u root -p table_name < file_name.sql
```  