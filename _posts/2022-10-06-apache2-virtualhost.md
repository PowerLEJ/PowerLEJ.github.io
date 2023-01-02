---
layout: single
title:  "Apache2 Virtual Host Setting"
categories: apache2
tag: apache2
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Apache2 Virtual Host Setting  

아파치 가상 호스트를 설정하면 하나의 서버에 여러 사이트를 운영할 수 있다.  

복사  
```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/001-default.conf
```  

수정  
```
sudo vim /etc/apache2/sites-available/001-default.conf
```  

```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName // 원하는 도메인주소 입력 ex) www.naver.com
    ServerAlias // 원하는 도메인주소 입력 ex) www.naver.com
    DocumentRoot /var/www/html // 원하는 경로로 수정
</VirtualHost>
```  

apache2.conf의  
```
sudo vim /etc/apache2/apache2.conf // 약 164번 줄에 <Directory /var/www/> 를 원하는 경로로 수정
```  

적용  
```
sudo a2ensite 001-default.conf
```  

취소는  
```
sudo a2dissite 001-default.conf
```  

저장 후 재시작  
```
sudo systemctl restart apache2
```  

cafe24 등에 가서 도메인 DNS 관리에서 도메인 주소와 IP를 등록한다.  