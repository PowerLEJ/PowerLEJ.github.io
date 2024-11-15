---
layout: single
title:  "Raspberry Ramdisk 라즈베리파이 램디스크 만들기"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 라즈베리파이 램디스크 만들기  
<br />

```
free -mega // 가용램용량확인
sudo mkdir /mnt/ramdisk
sudo mount -t tmpfs -o size=1G tmpfs /mnt/ramdisk
df -h // 마운트된 램디스크 확인
```  

```
sudo nano /etc/fstab // 이곳에서 아래 내용 추가
tmpfs /mnt/ramdisk tmpfs defaults,size=1G 0 0 // 앞으로 부팅 시마다 램디스크 자동으로 마운트 됨.
```  

mariadb-server 설치 후  

```
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf // 에서
datadir = /mnt/ramdisk/mysql // 로 수정한다
```  


나는 부팅과 동시에 할 일이 있어서  
```
sudo nano /etc/rc.local // 이 두 문장을 rc.local에 입력
sudo rsync -av /var/lib/mysql /mnt/ramdisk // 램디스크에 sql을 복사하는 기능
sudo systemctl restart mariadb // 디비를 재시작 하는 기능
```  

```
mysql -u root -p // 비번 치고
use mysql // 한 다음
SELECT @@datadir; // 으로 보면 /mnt/ramdisk/mysql/ 이라 되어 있음.
```  