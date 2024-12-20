---
layout: single
title:  "Raspberry Pi Class KeyPad & FND4 & Piezo & Servo [PWM]"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Piezo  
Physical pin 기준으로 18번을 피에조의 -를, 2번 5V에 피에조의 +를 연결  

![01_piezo](/images/2024-11-14-Raspberry_class/01_piezo.jpg){: width="100%" height="100%"}{: .center}  

## Servo  
Physical pin 기준으로 
18번을 Servo의 주황색 선에,  
2번 5V를 Servo의 빨간색 선에,  
39번 Ground를 Servo의 갈색선에 연결함  
![02_servo](/images/2024-11-14-Raspberry_class/02_servo.jpg){: width="100%" height="100%"}{: .center}  

## PWM (Pulse Width Modulation)  
펄스 변조의 일종으로 신호의 크기에 따라 펄스의 폭을 변조하는 방식  

![02_pwm_01](/images/2024-11-14-Raspberry_class/02_pwm_01.jpg){: width="100%" height="100%"}{: .center}  
  
- 아날로그 출력  
0.1V, 2.7V 등 출력 가능  

- 디지털 출력  
0 또는 1만 되니까 예를 들어 0V 또는 5V 만 출력 가능함.  
  
PWM을 사용하면 아날로그 출력 느낌을 할 수 있다.  
  
오실로스코프 : 특정 시간 간격(대역)의 전압 변화를 볼 수 있는 장치  
SMPS(Switching Mode Power Supply) : 스위칭 동작에 의한 전원을 공급해주는 장치  

![02_pwm_02](/images/2024-11-14-Raspberry_class/02_pwm_02.jpg){: width="100%" height="100%"}{: .center}  

## Ultrasonic Sensor 초음파 센서  

부품명 : HC-SR04  
  
- VCC // 5V // (Physical 기준 : 2)  
- Trig // OUTPUT 출력 // 발신 // (Physical 기준 : 16)  
- Echo // INPUT 입력 // 수신 // (Physical 기준 : 18)  
- GND // GND // (Physical 기준 : 39)  
  
마이크로초(microsecond, μs) or 마이크로세컨드 : 백만 분의 1초  
공식: uS / 58 = centimeters  

![20241115_ultrasonic](/images/2024-11-15-Raspberry_class/20241115_ultrasonic.jpg){: width="100%" height="100%"}{: .center}  