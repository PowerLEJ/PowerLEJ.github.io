---
layout: single
title:  "Arduino Basic"
categories: Arduino
tag: Arduino
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)
  
# 아두이노  
[아두이노](https://www.arduino.cc/) > SOFTWARE > 원하는 버전 Download  

## ~.tar.xz 를 풀어주고(오른쪽 마우스 > Extrac Here)  

```
cd ~/Downloads/arduino-1.8.19-linux64/arduino-1.8.19
sudo sh install.sh
```  

## Arduino IDE 켜고  
File > Preferences > Display line numbers, compilation, upload 체크하기  
Tools > Board: > Arduino AVR Boards > Arduio Uno  
Tools > Port > /dev/ttyACM0 (Arduio Uno)  
  
File > Examples > 01.Basics > Blink  
이 코드를 Verify, Upload 해서 잘 되는지 확인 가능  
  
우측 상단 돋보기 Serial Monitor  
input칸에 숫자 써서 Send 할 수 있음  

Serial.begin(9600); 이렇게 세팅했으면  
Serial Monitor 우측 하단에 9600 baud 로 맞춰야 함  

```c
const int pinLed = 13;

void setup() {
  pinMode(pinLed,OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if(Serial.available() > 0) {
    unsigned char ch = Serial.read();
    if     (ch == '1') digitalWrite(pinLed,HIGH);
    else if(ch == '0') digitalWrite(pinLed,LOW );
    else;
  }
}
```  

## 시리얼 포트 이름 확인
```
ls /dev/ttyA*

/*
ttyACM0
*/
```  

```
ll ttyACM0

/*
crw-rw---- 1 root dialout 166, 0 12월 26 14:35 ttyACM0
*/
```  


```
sudo chmod a+rw /dev/ttyACM0
# sudo chmod 666 /dev/ttyACM0
```  

```
ll ttyACM0

/*
crw-rw-rw- 1 root dialout 166, 0 12월 26 14:35 ttyACM0
*/
```  


```
id

/*
uid=1000(lej) gid=1000(lej) groups=1000(lej),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),133(lxd),134(sambashare)
*/
```  

```
sudo usermod -a -G dialout $USER
```  

리부팅 하고 보면 20(dialout)가 추가된 걸 볼 수 있다.  
```
id

/*
uid=1000(lej) gid=1000(lej) groups=1000(lej),4(adm),20(dialout),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),133(lxd),134(sambashare)
*/
```  