---
layout: single
title:  "Apache2, PHP, MariaDB, Nodejs, PM2 Install"
categories: apache2
tag: apache2
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Apache2, PHP, MariaDB Install  

```
sudo apt-get install vim
sudo apt-get install apache2
sudo add-apt-repository -y ppa:ondrej/php
sudo apt-get install php php-common libapache2-mod-php php-mysql php-xml php-curl php-mbstring php-zip php-json php-gd
// sudo apt install php7.2 php7.2-common php7.2-mysql php7.2-mbstring php7.2-curl php7.2-xml php7.2-json php7.2-gd php7.2-zip libapache2-mod-php7.2
// sudo apt install php8.2 php8.2-common php8.2-mysql php8.2-mbstring php8.2-curl php8.2-xml php8.2-json php8.2-gd php8.2-zip libapache2-mod-php8.2
sudo apt-get install mariadb-server
sudo mysql_secure_installation
```  

```
mysql -u root -p
MariaDB[(none)]> CREATE USER '아이디'@'localhost' IDENTIFIED BY '비번';
MariaDB[(none)]> GRANT ALL PRIVILEGES ON *.* TO '아이디'@'localhost';
MariaDB[(none)]> CREATE USER '아이디'@'%' IDENTIFIED BY '비번';
MariaDB[(none)]> GRANT ALL PRIVILEGES ON *.* TO '아이디'@'%';
MariaDB[(none)]> FLUSH PRIVILEGES;
MariaDB[(none)]> SELECT User, Host FROM mysql.user;
```  

### DB Connect 안될 때  
```
// 방법 1)
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf // 에서 주석하기
// bind-address = 127.0.0.1

// 방법 2)
sudo vim /etc/mysql/my.cnf // 에서 추가
[mysqld]
bind-address = 0.0.0.0
# skip-networking=0
# skip-bind-address

```  

저장 후 재시작  
```
sudo systemctl restart mariadb
```  

### 폴더, 파일 권한 바꾸기  
예시  
```
    chown -R www-data:www-data /home/folder
```  

### Apache2 웹 서버 기본 경로 설정  

```
sudo vi /etc/apache2/sites-available/000-default.conf 
// 에서 약 12번째 줄에 DocumentRoot /var/www/html 을 원하는 경로로 수정

sudo vi /etc/apache2/apache2.conf 
// 에서 약 164번 줄에 <Directory /var/www/> 를 원하는 경로로 수정
```  

저장 후 재시작  
```
sudo systemctl restart apache2
```  

### 포트 설정 방법  

```
sudo cp 000-default.conf 001-default.conf // 이런 식으로 복사 해서 수정
sudo vim /etc/apache2/sites-available/001-default.conf
sudo vim /etc/apache2/apache2.conf
sudo vim /etc/apache2/ports.conf
sudo a2ensite 001-default.conf
```  

취소는  
```
sudo a2dissite 001-default.conf
```  

재시작 후 확인  
```
sudo systemctl restart apache2
sudo netstat -tulpn | grep LISTEN
```  

### Log4php 로그 확인

[Apache log4php Click](https://logging.apache.org/log4php/){: .btn .btn--warning} 의 Download에서  
Apache log4php 파일 다운로드 한다.  
apache-log4php-2.3.0/src/main/php 폴더를 log4php로 이름 변경 후 최상단에 둔다. www-data여야 한다.  

로그 확인을 위해서는  
최상단/log4php 안에 log4php 관련 파일들이 있어야 하며  
로그 파일이 저장되는 폴더(ex. /home/컴퓨터이름/log4php)는 www-data여야 한다.  

### Node js Install  
PPA 를 이용하여 설치  

```
sudo curl -sL https://deb.nodesource.com/setup_버전.x | sudo -E bash -

// 예시: sudo curl -sL https://deb.nodesource.com/setup_15.x | sudo -E bash -

// 예시: sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```  

```
sudo apt-get install nodejs
```  

### PM2 Install  

```
npm install -g pm2
```  

New major version of npm available! 7.7.6 -> 9.2.0 이런 에러가 뜬다  

```
npm install -g npm@9.2.0
pm2 -version
```  

Cannot find module 'agentkeepalive' 이런 에러가 뜬다  

```
npm install -g agentkeepalive --save
```  

pm2 시작 시키기  

```
pm2 start 파일명 -i 갯수 --name "이름"
// 예시: pm2 start app.js -i 4 --name "app"
```  

pm2 삭제하기  

```
pm2 monit // 로 이름이나 숫자를 본다  
pm2 delete 이름이나 숫자
// 예시: pm2 delete 0 1 2
```  

pm2 리로드 또는 재시작 하기  

```
pm2 reload 이름  
// 예시: pm2 reload app  
pm2 restart 이름  
// 예시: pm2 restart app  
```  

### ProxyRequest 관련 에러  

```
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
```  

재시작 후 확인  
```
sudo systemctl restart apache2
```  