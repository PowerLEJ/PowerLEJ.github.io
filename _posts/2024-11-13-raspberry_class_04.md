---
layout: single
title:  "Raspberry Pi Class Switch & FND & KeyPad"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 폴링 방식 프로그램  
대상을 주기적으로 감시하여 상황을 발생하면 해당 처리 루틴을 실행해 처리  
ex. 친구를 집에 초대했는데 초인종도 없고 핸드폰도 없어서  
내가 식사도 준비하면서 친구가 왔는지 계속 문도 열어보고, 엘레베이터까지 내려갔다가 확인해야 하는 느낌.  

## 인터럽트 방식 프로그램  
새치기, 중단  
CPU가 프로그램을 실행하고 있을때, 입출력 어떠한 주변장치들의 입출력이나 하드웨어 문제가 발생하여 처리가 필요한 경우,  
프로그램에서 예외 등이 발생했을때 CPU에게 이를 알려주는 방식  
  
![raspberry_pin](/images/2024-11-05-Raspberry_class/raspberry_pin.png){: width="100%" height="100%"}{: .center}  

```
LED 0 : 16 (wPi)	10(phys)     A:07(FND)
LED 1 : 27 (wPi)	36(phys)     B:06(FND)
LED 2 :  0 (wPi)	11(phys)     C:04(FND)
LED 3 : 22 (wPi)	31(phys)     D:02(FND)
LED 4 : 24 (wPi)	35(phys)     E:01(FND)
LED 5 : 28 (wPi)	38(phys)     F:09(FND)
LED 6 : 26 (wPi)	32(phys)     G:10(FND)
LED 7 :  3 (wPi)	15(phys)    DP:05(FND)

SW0 :   10 (wPi)	24(phys)
SW1 :   11 (wPi)	26(phys)
SW2 :   31 (wPi)	28(phys)

FND_D1 : 19(phys)
FND_D2 : 21(phys)
FND_D3 : 23(phys)
FND_D4 : 27(phys)

KEYPAD_R1 : 12(phys)
KEYPAD_R2 : 13(phys)
KEYPAD_R3 : 16(phys)
KEYPAD_R4 : 18(phys)
KEYPAD_C1 : 22(phys)
KEYPAD_C2 : 24(phys)
KEYPAD_C3 : 26(phys)
KEYPAD_C4 : 28(phys)
```  
  
## 스위치  
![02_sw_draw](/images/2024-11-13-Raspberry_class/02_sw_draw.jpg){: width="100%" height="100%"}{: .center}  
  
![02_sw](/images/2024-11-13-Raspberry_class/02_sw.jpg){: width="100%" height="100%"}{: .center}  

## FND  
부품명 : 5611AH (공통캐소드)  
부품명 : 5161AS (공통캐소드)
![09_fnd](/images/2024-11-13-Raspberry_class/09_fnd.jpg){: width="100%" height="100%"}{: .center}  
  
![0910_fnd](/images/2024-11-13-Raspberry_class/0910_fnd.jpg){: width="100%" height="100%"}{: .center}  

## FND  
부품명 : 3461AS-1 (공통캐소드)
![10_fnd](/images/2024-11-13-Raspberry_class/10_fnd.jpg){: width="100%" height="100%"}{: .center}  

## KeyPad  
![10_fnd_2](/images/2024-11-13-Raspberry_class/10_fnd_2.jpg){: width="100%" height="100%"}{: .center}  
  
![12_keypad](/images/2024-11-13-Raspberry_class/12_keypad.jpg){: width="100%" height="100%"}{: .center}  