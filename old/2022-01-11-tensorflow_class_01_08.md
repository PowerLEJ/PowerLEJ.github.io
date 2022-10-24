---
layout: single
title:  "Tensorflow Class 01 08 DNN-GAN"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## GAN 기술 이론  
- 두 개의 인공 신경망이 서로 경쟁하며 학습하는 방식  
-- 생성자(Generator): 실제 데이터를 학습하고 이를 바탕으로 거짓 데이터를 생성 (도둑)  
-- 감별자(Discriminator): 생성자가 내놓은 거짓 데이터가 거짓임을 판별하도록 학습 (경찰)  

- 첫 번째 학습을 거침  
-- 생성자가 가짜 이미지 하나 생성해서 감별자에게 줌  
-- 감별자가 가짜라고 감별하면 실패: 생성자 가중치 보정  
-- 감별자가 진짜라고 속으면 성공  
-- [주의점]  
-- 생성자의 가중치는 가변  
-- 감별자의 가중치는 고정  

- 두 번째 학습을 거침  
-- 생성자가 가짜 이미지 하나 생성  
-- 이 가짜 이미지를 감별자에게: 가짜로 판별하도록 학습  
-- 진품 창고에서 진짜 이미지 하나 임의로 추출  
-- 이 진짜 이미지를 감별자에게: 진짜로 판별하도록 학습  

- GAN 손실 함수  
-- 이 함수롤 최소화 하자  
![tensor_class_01_08_01](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_01.png){: width="100%" height="100%"}{: .center}  

-- D(G(z(i))): 감별자가 i번째 가짜를 감별한 결과: 0은 가짜, 1은 진짜  
-- 생성자는 감별자를 속이기 위해 학습  
-- 왜 log를 사용하는지? 확률 변수를 다루기 용이해서  
Log(x*y) = log(x) + log(y)  
-- ▽(편미분): 기울기를 계산해서 경사하강법으로 보정  

-- 이 함수를 최대화 하자  
![tensor_class_01_08_02](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_02.png){: width="100%" height="100%"}{: .center}  
-- D(x(i)): 감별자가 i번째 진짜를 감별한 결과: 0은 가짜, 1은 진짜  
-- D(G(z(i))): 감별자가 i번째 가짜를 감별한 결과: 0은 가짜, 1은 진짜  
-- 진짜를 보고 판단한 확률은 1에 가까워야 함  
-- 가짜를 보고 판단한 확률은 0에 가까워야 함  

## MNIST 손글씨 Dataset  
- 기계학습에 가장 많이 사용되는 Dataset  
-- 미국 인구조사에 쓰인 손글씨  
-- NIST(미국 국립 표준기술 연구소)에서 공개  
-- 6만개의 학습용 데이터, 1만개의 평가용 데이터 존재  
-- 흑백 이미지 데이터  

![tensor_class_01_08_03](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_03.png){: width="100%" height="100%"}{: .center}  

## 패키지 설치
- numpy, matplotlib, keras  

```python
# 파이썬 패키지 가져오기
import matplotlib.pyplot as plt
import numpy as np
from time import time
import os
import glob

from keras.datasets import mnist
from keras.layers import Dense, Flatten, Reshape
from keras.layers import LeakyReLU
from keras.models import Sequential

# 하이퍼 파라미터
MY_GEN = 128 # 생성자에 들어 있는 은닉층 각각의 뉴런의 숫자
MY_DIS = 128 # 감별자에 들어 있는 은닉층 각각의 뉴런의 숫자
MY_NOISE = 100 # 생성자가 가짜 데이터를 만들 때 입력으로 사용하는 노이즈 백터 숫자

MY_SHAPE = (28, 28, 1) # 손글씨 이미지의 모양, 흑백이고 화소수는 28*28, 채널은 1
MY_EPOCH = 5000
MY_BATCH = 300

# 출력 이미지 폴더 생성
MY_FOLDER = 'output/'
os.makedirs(MY_FOLDER,
            exist_ok=True)

for f in glob.glob(MY_FOLDER + '*'):
    os.remove(f)


############### 데이터 준비 ##############

# 결과는 numpy의 n-차원 행렬 형식
def red_data():
    # 학습용 입력값만 사용 (GAN은 비지도 학습이기 때문에 출력 데이터, 평가용도 필요 없음)
    (X_train, _), (_, _) = mnist.load_data()

    print('데이터 모양:', X_train.shape)
    plt.imshow(X_train[0], cmap='gray')
    plt.show()

    # [-1, 1] 데이터 스케일링
    X_train = X_train / 127.5 - 1.0

    # 채널 정보 추가
    X_train = np.expand_dims(X_train, axis=3)  # 차원 확장 함수 expand_dims
    print('데이터 모양:', X_train.shape)

    return X_train

red_data()
```  

>  
결과  
```
    데이터 모양: (60000, 28, 28)
    데이터 모양: (60000, 28, 28, 1)
```  

![tensor_class_01_08_04](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_04.png){: width="100%" height="100%"}{: .center}  

## Leaky ReLu 활성화 함수  
- ReLU 함수에서 파생  
-- ReLU의 단점인 죽는 뉴런을 방지하는 효과  
-- 기울기 유실 문제 부분적 해결  
-- 음수 쪽의 기울기 조정 가능  
![tensor_class_01_08_05](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_05.png){: width="100%" height="100%"}{: .center}  

### 생성자 구현  
- 감별자를 속이는 것이 목적  
-- 입력: 노이즈 벡터, 출력: 가짜 이미지 하나  
![tensor_class_01_08_06](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_06.png){: width="100%" height="100%"}{: .center}  

```python
################ 인공 신경망 구현 #################

# 생성자 설계
def build_generator():
    model = Sequential()

    # 입력층 + 은닉층 1
    model.add(Dense(MY_GEN,
                    input_dim=MY_NOISE))
    model.add(LeakyReLU(alpha=0.01))

    # 은닉층 2
    model.add(Dense(MY_GEN))
    model.add(LeakyReLU(alpha=0.01))

    # 은닉층 3 + 출력층
    # tanh 활성화는 [-1, 1] 스케일링 때문
    model.add(Dense(28 * 28 * 1,
                    activation='tanh'))
    model.add(Reshape(MY_SHAPE))

    print('\n생성자 요약')
    model.summary()

    return model

build_generator()
```  

>  
결과  
```
    생성자 요약
    Model: "sequential"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    dense (Dense)               (None, 128)               12928     
    leaky_re_lu (LeakyReLU)     (None, 128)               0         
    dense_1 (Dense)             (None, 128)               16512     
    leaky_re_lu_1 (LeakyReLU)   (None, 128)               0         
    dense_2 (Dense)             (None, 784)               101136    
    reshape (Reshape)           (None, 28, 28, 1)         0         
    =================================================================
    Total params: 130,576
    Trainable params: 130,576
    Non-trainable params: 0
    _________________________________________________________________
```  

### 감별자 구현  
- 생성자의 가짜 이미지를 감별하는 것이 목적  
-- 입력: 이미지(가짜/진짜), 출력: 감별 결과(확률)
![tensor_class_01_08_07](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_07.png){: width="100%" height="100%"}{: .center}  

```python
# 감별자 설계
def build_discriminator():
    model = Sequential()

    # 입력층
    model.add(Flatten(input_shape=MY_SHAPE))

    # 은닉층 1
    model.add(Dense(MY_DIS))
    model.add(LeakyReLU(alpha=0.01))

    # 출력층
    model.add(Dense(1,
                    activation='sigmoid'))

    print('\n감별자 요약')
    model.summary()

    return model

build_discriminator()
```  

>  
결과  
```
    감별자 요약
    Model: "sequential"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    flatten (Flatten)           (None, 784)               0         
    dense (Dense)               (None, 128)               100480    
    leaky_re_lu (LeakyReLU)     (None, 128)               0         
    dense_1 (Dense)             (None, 1)                 129       
    =================================================================
    Total params: 100,609
    Trainable params: 100,609
    Non-trainable params: 0
    _________________________________________________________________
```  

## DNN-GAN 설계도  
- 생성자와 감별자 연결  
-- 입력: 노이즈 벡터(생성자의 입력)  
-- 출력: 감별 결과(감별자의 출력)  
-- 생성자의 출력이 감별자의 입력이 됨  
![tensor_class_01_08_08](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_08.png){: width="100%" height="100%"}{: .center}  


```python
# DNN-GAN 구현
def build_GAN():
    model = Sequential()

    # 생성자 구현
    generator = build_generator()

    # 감별자 구현
    # 생성자 학습 시 감별자 고정
    discriminator = build_discriminator()

    discriminator.compile(optimizer='adam',
                          loss='binary_crossentropy',
                          metrics=['acc'])

    discriminator.trainable = False

    # GAN 구현: 생성자 먼저 추가, 그 다음 감별자
    model.add(generator)
    model.add(discriminator)

    # GAN은 정확도 무의미
    model.compile(optimizer='adam',
                  loss='binary_crossentropy')

    print('\nGAN 요약')
    model.summary()

    return discriminator, generator, model

build_GAN()
```  

>  
결과  
```
    생성자 요약
    Model: "sequential_1"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    dense (Dense)               (None, 128)               12928     
    leaky_re_lu (LeakyReLU)     (None, 128)               0         
    dense_1 (Dense)             (None, 128)               16512     
    leaky_re_lu_1 (LeakyReLU)   (None, 128)               0         
    dense_2 (Dense)             (None, 784)               101136    
    reshape (Reshape)           (None, 28, 28, 1)         0         
    =================================================================
    Total params: 130,576
    Trainable params: 130,576
    Non-trainable params: 0
    _________________________________________________________________
    감별자 요약
    Model: "sequential_2"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    flatten (Flatten)           (None, 784)               0         
    dense_3 (Dense)             (None, 128)               100480    
    leaky_re_lu_2 (LeakyReLU)   (None, 128)               0         
    dense_4 (Dense)             (None, 1)                 129       
    =================================================================
    Total params: 100,609
    Trainable params: 100,609
    Non-trainable params: 0
    _________________________________________________________________
    GAN 요약
    Model: "sequential"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    sequential_1 (Sequential)   (None, 28, 28, 1)         130576    
    sequential_2 (Sequential)   (None, 1)                 100609    
    =================================================================
    Total params: 231,185
    Trainable params: 130,576
    Non-trainable params: 100,609
    _________________________________________________________________
```

## DNN_GAN 역전파 방식  
![tensor_class_01_08_09](/images/2022-01-11-tensorflow_class_01_08/tensor_class_01_08_09.png){: width="100%" height="100%"}{: .center}  



임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
