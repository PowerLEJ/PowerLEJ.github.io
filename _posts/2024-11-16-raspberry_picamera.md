---
layout: single
title:  "Raspberry Pi Picamera2"
categories: Raspberry
tag: Raspberry
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Picamera2(libcamera)  

5초 정도 확인 가능  
```
libcamera-hello
```  

jpg 파일로 저장  
```
libcamera-jpeg -o test.jpg
```  

[출처: 라즈베리파이](https://github.com/raspberrypi/picamera2/blob/main/examples/mjpeg_server.py){: .btn .btn--warning}

```python
#!/usr/bin/python3

# Mostly copied from https://picamera.readthedocs.io/en/release-1.13/recipes2.html
# Run this script, then point a web browser at http:<this-ip-address>:8000
# Note: needs simplejpeg to be installed (pip3 install simplejpeg).

import io
import logging
import socketserver
from http import server
from threading import Condition

from picamera2 import Picamera2
from picamera2.encoders import JpegEncoder
from picamera2.outputs import FileOutput

PAGE = """\
<html>
<head>
<title>picamera2 MJPEG streaming demo</title>
</head>
<body>
<h1>Picamera2 MJPEG Streaming Demo</h1>
<img src="stream.mjpg" width="640" height="480" />
</body>
</html>
"""


class StreamingOutput(io.BufferedIOBase):
    def __init__(self):
        self.frame = None
        self.condition = Condition()

    def write(self, buf):
        with self.condition:
            self.frame = buf
            self.condition.notify_all()


class StreamingHandler(server.BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/':
            self.send_response(301)
            self.send_header('Location', '/index.html')
            self.end_headers()
        elif self.path == '/index.html':
            content = PAGE.encode('utf-8')
            self.send_response(200)
            self.send_header('Content-Type', 'text/html')
            self.send_header('Content-Length', len(content))
            self.end_headers()
            self.wfile.write(content)
        elif self.path == '/stream.mjpg':
            self.send_response(200)
            self.send_header('Age', 0)
            self.send_header('Cache-Control', 'no-cache, private')
            self.send_header('Pragma', 'no-cache')
            self.send_header('Content-Type', 'multipart/x-mixed-replace; boundary=FRAME')
            self.end_headers()
            try:
                while True:
                    with output.condition:
                        output.condition.wait()
                        frame = output.frame
                    self.wfile.write(b'--FRAME\r\n')
                    self.send_header('Content-Type', 'image/jpeg')
                    self.send_header('Content-Length', len(frame))
                    self.end_headers()
                    self.wfile.write(frame)
                    self.wfile.write(b'\r\n')
            except Exception as e:
                logging.warning(
                    'Removed streaming client %s: %s',
                    self.client_address, str(e))
        else:
            self.send_error(404)
            self.end_headers()


class StreamingServer(socketserver.ThreadingMixIn, server.HTTPServer):
    allow_reuse_address = True
    daemon_threads = True


picam2 = Picamera2()
picam2.configure(picam2.create_video_configuration(main={"size": (640, 480)}))
output = StreamingOutput()
picam2.start_recording(JpegEncoder(), FileOutput(output))

try:
    address = ('', 8000)
    server = StreamingServer(address, StreamingHandler)
    server.serve_forever()
finally:
    picam2.stop_recording()
```  

라즈베리파이의 IP주소:8000 에서 html 확인 가능  

## opencv  

### 설치  
```
sudo apt-get update
sudo apt-get install python3-opencv
```  

### 확인  
```
dpkg -l | grep python3-opencv
```  

```python
import cv2
print(cv2.__version__)
```  

## Raspberry Pi OS Debian version: 12 Bookworm  

|특징|Legacy (Bullseye)|Bookworm|
|:---:|:---:|:---:|
|카메라 API|VideoCapture(0)|Picamera2 (libcamera)|
|디폴트 색상 공간|BGR|YUV420 (수동 설정 필요)|
|하드웨어 가속|GPU/MMAL|libcamera (PiCamera2가 대체)|
|코드 수정|간단한 OpenCV VideoCapture 사용|Picamera2 설정 및 초기화 필요|

```python
from picamera2 import Picamera2, Preview # Picamera2: Raspberry Pi의 PiCamera2 API를 사용해 카메라 모듈을 제어
import cv2 # cv2 (OpenCV): OpenCV를 사용하여 이미지 처리를 수행
import numpy as np # numpy: 이미지 데이터 처리를 위해 NumPy를 사용

picam2 = Picamera2() # Picamera2 객체 생성: PiCamera2 API를 사용하기 위해 카메라 객체를 초기화

# 이미지 해상도 설정: 640x480 크기의 이미지를 캡처하도록 설정
# format="RGB888": OpenCV에서 컬러 이미지를 올바르게 처리할 수 있도록 RGB888 포맷으로 설정
camera_config = picam2.create_still_configuration(
    main={"size": (640, 480), "format": "RGB888"}  # Ensure correct color format
)
picam2.configure(camera_config)

picam2.start() # 카메라 스트리밍을 시작

# OpenCV에서 영상을 표시할 윈도우를 생성
cv2.namedWindow("Picamera2 OpenCV Example", cv2.WINDOW_AUTOSIZE) # cv2.WINDOW_AUTOSIZE: 윈도우 크기를 영상 크기에 맞게 조정

while True:
    
    frame = picam2.capture_array() # picam2.capture_array(): 카메라에서 프레임을 NumPy 배열로 가져옴
    # frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY) # 흑백으로 변환

    cv2.imshow("Picamera2 OpenCV Example", frame) # cv2.imshow(): 프레임을 OpenCV 창에 표시

    # cv2.waitKey(1): 키 입력 대기. 'q' 키를 누르면 루프가 종료
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cv2.destroyAllWindows() # cv2.destroyAllWindows(): OpenCV 창을 닫기
picam2.stop() # picam2.stop(): 카메라 스트리밍을 중지
```  


