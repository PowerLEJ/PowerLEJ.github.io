---
layout: single
title:  "Tensorflow Class 01 03"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## ReLU 활성화 함수
- Rectified Linear Unit  
-- 수학적 정의: F(x) = max(0, x)  
-- 음수를 0으로 만듦  
-- 도움이 되지 않는 뉴런을 비활성화 함  
-- 기울기 소실 문제가 적음  

```python
######## 인공 신경망 구현 ########

# 케라스 DNN 구현
model = Sequential()
input = X_train.shape[1] # (354, 12) 에서 1번째는 12이다
# 은닉층 1 Dense는 은닉층에 대한 뉴런 200개랑 입력층의 12개 뉴런을 곱해서 2400개 그리고 은닉층 뉴런 200개 더해서 시냅스 총 2600개
model.add(Dense(200,
                input_dim=input,
                activation='relu'))
# 은닉층 2추가
model.add(Dense(1000,
                activation='relu'))
# 최종 출력층 추가
model.add(Dense(1))

print('\nDNN 요약')
model.summary()
```  

>  
결과  
```
    DNN 요약
    Model: "sequential"
                                # None 은 batch size이고 바뀔 수 있기 때문에 모른다는 표현
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    dense (Dense)               (None, 200)               2600      
    dense_1 (Dense)             (None, 1000)              201000     # 1000개 뉴런이 은닉층 1의 200개 뉴런과 덴스 연결 해서 시냅스 20만개
    dense_2 (Dense)             (None, 1)                 1001       # 1개 뉴런이 은닉층 2의 1000개 뉴런과 덴스 연결 해서 시냅스 1000개
    =================================================================
    Total params: 204,601                     # 2600 + 201000 + 1001 = 204601
    Trainable params: 204,601
    Non-trainable params: 0
    _________________________________________________________________
```  

## 경사하강법 이론  
- Stochastic Gradient Descent (SGD)  
-- 지도학습에서 가중치를 보정하는 대표적 방법  
-- 함수의 기울기(경사)를 구하여 낮은 쪽으로 이동  

![tensor_class_01_03_01](/images/2022-01-07-tensorflow_class_01_03/tensor_class_01_03_01.png){: width="100%" height="100%"}{: .center}



임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
