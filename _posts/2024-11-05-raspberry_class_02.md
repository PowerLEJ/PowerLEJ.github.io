---
layout: single
title:  "Raspberry Pi 4 WiringPi Install"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## WiringPi Install  

나는 cd Desktop에 클론해왔다.  
```
git clone https://github.com/WiringPi/WiringPi
```  

```
cd WiringPi
./build
```  

### 명령  

```
gpio -v
gpio readall
```  

![gpio_readall](/images/2024-11-05-Raspberry_class/gpio_readall.png){: width="50%" height="100%"}{: .center}  
  
![raspberry_pin](/images/2024-11-05-Raspberry_class/raspberry_pin.png){: width="50%" height="100%"}{: .center}  
  
![led_sw_num](/images/2024-11-05-Raspberry_class/led_sw_num.png){: width="50%" height="100%"}{: .center}  