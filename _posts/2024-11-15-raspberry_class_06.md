---
layout: single
title:  "Raspberry Pi Class 통신"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 통신  

데이터 전달 방식에 따라 직렬 통신과 병렬 통신으로 구분  

### 직렬 통신  
한번에 하나의 데이터 비트를 전송  
  
- UART: 병렬 데이터를 직렬화 하여 통신하는 개별 집적 회로.  
비동기 통신이므로 동기 신호가 전달되지 않음  
따라서 수신 쪽에서 동기신호를 찾아내어 데이터의 시작과 끝을 시간적으로 알아 처리할 수 있도록 약속함  
전이중방식. 일대일 통신  
- I2C 통신(IIC) : 동기식 직렬통신. 양방향(반이중). 선 2개. 일대다통신.  
master slave (만약 마스터들이 동시에 start신호를 보내면 낮은 slave address를 보낸 master가 우선순위를 가짐)
- SPI 통신 : 동기식 직렬통신. 양방향(전이중, 전이중)  
- CAN 통신 : 비동기식 직렬 통신  

### 병렬 통신  
한번에 여러 개의 데이터 비트를 전송  

MSB (Most Significant Bit) : 데이터 형의 최상위 비트  
LSB (Least Significant Bit) : 데이터 형의 최하위 비트  

<hr />

### 양방향 통신

#### 반이중(Half-Duplex)  
정보의 전송이 양방향이지만, 한쪽이 송신하는 동안은 다른 쪽이 송신하지 못하는 방식  
ex. 무전기  

#### 전이중(Full-Duplex)  
정보의 전송이 양방향으로 이루어지고 양쪽 모두 동시에 송신이 가능한 방식  
유선인 경우 4선 식으로 구성  
ex. 전화기  

### 단방향 통신
정보의 전송이 한 방향으로만 이루어지는 방식  

<hr />

### 동기 통신  
약속

### 비동기 통신  
비약속

<hr />

### 일대일 통신  
### 일대다 통신  
### 다대다 통신  

<hr />
  
## 연결  

![20241115_uart_memo](/images/2024-11-15-Raspberry_class/20241115_uart_memo.jpg){: width="100%" height="100%"}{: .center}  


- RS232 레벨 (PC)  
0 : -12V  
1 : 12V  
  
USB-시리얼 변환 장치를 사용하면 라즈베리와 PC가 통신할 수 있다 (ex. FTDI232)  
  
- TTL 레벨 (라즈베리파이, 아두이노)  
0 : 0V  
1 : 3.3V  

### 라즈베리파이_1와 라즈베리파이_2 간 연결  
Physical 기준 8번 : TX  
Physical 기준 10번 : RX  
  
라즈베리파이_1의 TX와 라즈베리파이_2의 RX 연결  
라즈베리파이_1의 RX와 라즈베리파이_2의 TX 연결  
라즈베리파이_1의 GND와 라즈베리파이_2의 GND 연결  

#### 터미널에서 하는 방법  
```
sudo raspi-config
```  
3 Interface Options > I4, I5, I6 을 Yes  

#### VNC에서 하는 방법  
Raspberry Pi Configuration > Interfaces >  
SPI: ON, I2C : ON, Serial Port : ON, Serial Console : OFF  



