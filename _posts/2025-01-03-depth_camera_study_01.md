---
layout: single
title:  "RealSense Depth Camera D435i Intel 기본"
categories: depth_camera
tag: depth_camera
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)
  
## RealSense SDK Install  

참고해서 설치하기  
[IntelRealSense.librealsense](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md)  

```
sudo mkdir -p /etc/apt/keyrings
curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null
```  

```
echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo `lsb_release -cs` main" | \
sudo tee /etc/apt/sources.list.d/librealsense.list
sudo apt-get update
```  

```
sudo apt-get install librealsense2-dkms
```  

```
sudo apt-get install librealsense2-utils
```  

```
sudo apt-get install librealsense2-dev
```  

```
sudo apt-get install librealsense2-dbg
```  

확인  
```
realsense-viewer
```  

![20250103_01](/images/2025-01-03-DepthCamera_study/20250103_01.png){: width="70%" height="70%"}{: .center}  

## RealSense ROS package Install  

참고  
[IntelRealSense.realsense-ros](https://github.com/IntelRealSense/realsense-ros)  

```
sudo apt install ros-humble-librealsense2*
```  

```
sudo apt install ros-humble-realsense2-*
```  

<br />

만약에 안되면 이것도 하고 (안해도 됨)  
```
# sudo apt install ros-humble-diagnostic-*
```  

### 1번 터미널에서  
```
ros2 launch realsense2_camera rs_launch.py depth_module.profile:=1280x720x30 pointcloud.enable:=true device_type:=d435
```  

### 2번 터미널에서  
```
rviz2
```  

![20250103_02](/images/2025-01-03-DepthCamera_study/20250103_02.png){: width="70%" height="70%"}{: .center}  