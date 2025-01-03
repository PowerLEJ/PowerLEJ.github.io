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

## Opencv with Python 

```
sudo apt install python3-pip
``` 

```
pip install opencv-python
``` 

```
pip install pyrealsense2
``` 

```
pip install numpy
``` 

## python으로 RealSense 연결  

```python
import pyrealsense2 as rs  # Intel RealSense SDK 모듈
import numpy as np         # 행렬 및 배열 계산을 위한 라이브러리
import cv2                 # OpenCV 라이브러리, 이미지 처리 및 GUI 제공


# RealSense 파이프라인 초기화
pipe = rs.pipeline()       # RealSense 데이터를 관리할 파이프라인 생성
cfg = rs.config()          # 스트림 설정을 위한 구성 객체 생성


# 컬러 스트림 설정 (해상도: 640x480, 형식: BGR, FPS: 30)
cfg.enable_stream(rs.stream.color, 640, 480, rs.format.bgr8, 30)


# 깊이 스트림 설정 (해상도: 640x480, 형식: Z16, FPS: 30)
cfg.enable_stream(rs.stream.depth, 640, 480, rs.format.z16, 30)


# 설정한 스트림으로 파이프라인 시작
pipe.start(cfg)


# 초기 포인트 설정 (화면 중앙 좌표)
point = (400, 300)


# 마우스 이벤트 콜백 함수
def show_distance(event, x, y, args, params):
   """
   마우스 클릭 시 클릭한 위치 좌표를 업데이트
   """
   global point
   point = (x, y)  # 현재 마우스 좌표로 업데이트


# OpenCV 창 생성 및 마우스 콜백 함수 등록
cv2.namedWindow("rgb frame")  # 컬러 이미지를 표시할 창 생성
cv2.setMouseCallback("rgb frame", show_distance)  # "rgb frame" 창에서 마우스 이벤트 처리


# 메인 루프
while True:
   # 프레임 가져오기 (깊이 프레임과 컬러 프레임)
   frame = pipe.wait_for_frames()
   depth_frame = frame.get_depth_frame()
   color_frame = frame.get_color_frame()


   # 깊이 및 컬러 데이터를 NumPy 배열로 변환
   depth_image = np.asanyarray(depth_frame.get_data())
   color_image = np.asanyarray(color_frame.get_data())


   # 깊이 이미지를 시각화하기 위해 컬러맵 적용
   depth_cm = cv2.applyColorMap(cv2.convertScaleAbs(depth_image, alpha=0.5), cv2.COLORMAP_JET)


   # 현재 포인트에 빨간 원 그리기
   cv2.circle(color_image, point, 4, (0, 0, 255))  # 포인트 좌표에 원 표시
   print(point)  # 현재 포인트 좌표 출력


   # 깊이 값 가져오기 (현재 포인트의 깊이값)
   distance = depth_image[point[1], point[0]]  # y, x 순서로 접근
   print(distance)  # 깊이값 출력 (밀리미터 단위)


   # 깊이값을 컬러 이미지에 텍스트로 표시
   cv2.putText(
       color_image,
       "{}mm".format(distance),  # 깊이값(mm 단위) 표시
       (point[0], point[1]),    # 텍스트 위치 (현재 포인트 좌표)
       cv2.FONT_HERSHEY_PLAIN,  # 폰트 스타일
       2,                       # 폰트 크기
       (0, 0, 0),               # 텍스트 색상 (검정색)
       2                        # 텍스트 두께
   )


   # 컬러 이미지 및 깊이 이미지 표시
   cv2.imshow('rgb frame', color_image)  # 컬러 이미지 표시
   cv2.imshow('depth frame', depth_cm)  # 컬러맵이 적용된 깊이 이미지 표시


   # 키 입력 대기: 'q'를 누르면 종료
   if cv2.waitKey(1) == ord('q'):
       break


# 파이프라인 정리 및 리소스 해제
pipe.stop()
```  

![20250103_03](/images/2025-01-03-DepthCamera_study/20250103_03.png){: width="70%" height="70%"}{: .center}  