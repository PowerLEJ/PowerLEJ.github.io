---
layout: single
title:  "Tensorflow Class 01 05 CNN"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 패키지 설치
- numpy, matplotlib, keras, scikit-learn  

## Numpy와 Pandas  
### Numpy  
- 데이터 차원: N-차원  
- 데이터 타입: 숫자 타입 하나만 허용  
- indexing: 숫자로만 가능  
- 빠른 과학, 공학적 계산이 목적  
-- 3차원 Numpy의 모양 정보는 (층의 수, 행의 수, 열의 수)  

### Pandas  
- 데이터 차원: 2-차원  
- 데이터 타입: 다양한 타입 혼용 가능  
- indexing: 숫자와 문자로 가능  
- 데이터 다양성 추구, 사용의 편리가 목적  

```python
# 파이썬 패키지 수입
import numpy as np
import matplotlib.pyplot as plt
from time import time

from keras.datasets import fashion_mnist
from keras.utils.np_utils import to_categorical
from sklearn.metrics import f1_score, confusion_matrix
from keras.models import Sequential
from keras.layers import Flatten
from keras.layers import Dense, InputLayer
from keras.layers import Conv2D, MaxPool2D

# 하이퍼 파라미터
MY_EPOCH = 3
MY_BATCH = 300

################### 데이터 준비 ###################

# 데이터 파일 읽기
# 결과는 numpy의 n-차원 행렬 형식
(X_train, Y_train), (X_test, Y_test) = fashion_mnist.load_data()
```  

Fasion MNIST Dataset을 AWS에서 다운로드 받음   
>  
결과  
```
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-labels-idx1-ubyte.gz
    32768/29515 [=================================] - 0s 0us/step
    40960/29515 [=========================================] - 0s 0us/step
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-images-idx3-ubyte.gz
    26427392/26421880 [==============================] - 3s 0us/step
    26435584/26421880 [==============================] - 3s 0us/step
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-labels-idx1-ubyte.gz
    16384/5148 [===============================================================================================] - 0s 0us/step
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-images-idx3-ubyte.gz
    4423680/4422102 [==============================] - 0s 0us/step
    4431872/4422102 [==============================] - 0s 0us/step
```  

```python
# 4분할 된 데이터 모양 출력
print('\n학습용 입력 데이터 모양: ', X_train.shape)
print('학습용 출력 데이터 모양: ', Y_train.shape)
print('평가용 입력 데이터 모양: ', X_test.shape)
print('평가용 출력 데이터 모양: ', Y_test.shape)
```  

>  
결과  
```
    학습용 입력 데이터 모양:  (60000, 28, 28)
    학습용 출력 데이터 모양:  (60000,)
    평가용 입력 데이터 모양:  (10000, 28, 28)
    평가용 출력 데이터 모양:  (10000,)
```  

```python
# 샘플 데이터 출력
print(X_train[0])
plt.imshow(X_train[0], cmap='gray')
plt.show()
print('샘플 데이터 라벨: ', Y_train[0])
```

>  
결과  
```
    [[  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
        0   0   0   0   0   0   0   0   0   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
        0   0   0   0   0   0   0   0   0   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
        0   0   0   0   0   0   0   0   0   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   1   0   0  13  73   0
        0   1   4   0   0   0   0   1   1   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   3   0  36 136 127  62
    54   0   0   0   1   3   4   0   0   3]
    [  0   0   0   0   0   0   0   0   0   0   0   0   6   0 102 204 176 134
    144 123  23   0   0   0   0  12  10   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0   0 155 236 207 178
    107 156 161 109  64  23  77 130  72  15]
    [  0   0   0   0   0   0   0   0   0   0   0   1   0  69 207 223 218 216
    216 163 127 121 122 146 141  88 172  66]
    [  0   0   0   0   0   0   0   0   0   1   1   1   0 200 232 232 233 229
    223 223 215 213 164 127 123 196 229   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0 183 225 216 223 228
    235 227 224 222 224 221 223 245 173   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0 193 228 218 213 198
    180 212 210 211 213 223 220 243 202   0]
    [  0   0   0   0   0   0   0   0   0   1   3   0  12 219 220 212 218 192
    169 227 208 218 224 212 226 197 209  52]
    [  0   0   0   0   0   0   0   0   0   0   6   0  99 244 222 220 218 203
    198 221 215 213 222 220 245 119 167  56]
    [  0   0   0   0   0   0   0   0   0   4   0   0  55 236 228 230 228 240
    232 213 218 223 234 217 217 209  92   0]
    [  0   0   1   4   6   7   2   0   0   0   0   0 237 226 217 223 222 219
    222 221 216 223 229 215 218 255  77   0]
    [  0   3   0   0   0   0   0   0   0  62 145 204 228 207 213 221 218 208
    211 218 224 223 219 215 224 244 159   0]
    [  0   0   0   0  18  44  82 107 189 228 220 222 217 226 200 205 211 230
    224 234 176 188 250 248 233 238 215   0]
    [  0  57 187 208 224 221 224 208 204 214 208 209 200 159 245 193 206 223
    255 255 221 234 221 211 220 232 246   0]
    [  3 202 228 224 221 211 211 214 205 205 205 220 240  80 150 255 229 221
    188 154 191 210 204 209 222 228 225   0]
    [ 98 233 198 210 222 229 229 234 249 220 194 215 217 241  65  73 106 117
    168 219 221 215 217 223 223 224 229  29]
    [ 75 204 212 204 193 205 211 225 216 185 197 206 198 213 240 195 227 245
    239 223 218 212 209 222 220 221 230  67]
    [ 48 203 183 194 213 197 185 190 194 192 202 214 219 221 220 236 225 216
    199 206 186 181 177 172 181 205 206 115]
    [  0 122 219 193 179 171 183 196 204 210 213 207 211 210 200 196 194 191
    195 191 198 192 176 156 167 177 210  92]
    [  0   0  74 189 212 191 175 172 175 181 185 188 189 188 193 198 204 209
    210 210 211 188 188 194 192 216 170   0]
    [  2   0   0   0  66 200 222 237 239 242 246 243 244 221 220 193 191 179
    182 182 181 176 166 168  99  58   0   0]
    [  0   0   0   0   0   0   0  40  61  44  72  41  35   0   0   0   0   0
        0   0   0   0   0   0   0   0   0   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
        0   0   0   0   0   0   0   0   0   0]
    [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
        0   0   0   0   0   0   0   0   0   0]]
    샘플 데이터 라벨:  9
```

![tensor_class_01_05_01](/images/2022-01-09-tensorflow_class_01_05/tensor_class_01_05_01.png){: width="100%" height="100%"}{: .center}  

## MinMax 정규화 이론  
- [0, 1] **스케일링**  
-- 모든 데이터의 값을 [0, 1] 범위로 전환  
-- 경사하강법 실행시간을 줄여줌  

```
X scaled = (X - X min) / (X max - X min)
```  

- Fasion MNIST 데이터  
-- Xmax = 255, Xmin = 0  
-- 각 화소를 255로 나누는 효과  

## CNN 차원 정보 추가  
- 이미지 데이터에서 채널의 의미  
-- 흑백 이미지: 채널은 1  
-- 칼라 이미지: 채널은 3 (red, green, blue)  

- Conv2D 입력 데이터 형식 (채널 정보를 요구함)  
-- 4차원 텐서 요구  
-- Channel-last 방식: batch + (rows, cols, channels)  

- Conv2D 출력 데이터 형식  
-- 4차원 텐서 출력  
-- Channel-last 방식: batch + (rows, cols, filters)  

## 원핫 인코딩 이론  
- 범주형 데이터 (Categorical data) 대상  
-- 데이터를 하나의 정수가 아닌, 여러 개의 2진수로 표현  
```
메이터   숫자   1-hot
기아     0    [1, 0, 0, 0]
현대     1    [0, 1, 0, 0]
벤츠     2    [0, 0, 1, 0]
혼다     3    [0, 0, 0, 1]
```  
-- 정수 데이터는 의도치 않은 혼동을 초래한다  
예시에서 CNN 예측값으로 '현대 1'와 '혼다 3'의 평균은 '벤츠 2'다??
-- One-hot Encoding은 이런 문제를 해결한다  
예시에서 CNN이 예측값을 1개를 내는 것이 아니라 4개를 낸다(출력 숫자가 하나가 아니고 넷)  

```python
# 입력 데이터 스케일링: [0, 1]
X_train = X_train / 255.0
X_test = X_test / 255.0

# 채널 정보 추가
# 케라스 CNN에서 4차원 정보 필요
train = X_train.shape[0]
X_train = X_train.reshape(train, 28, 28, 1) # (batch, rows, cols, channels) # 채널1=흑백이미지
test = X_test.shape[0]
X_test = X_test.reshape(test, 28, 28, 1) # (batch, rows, cols, filters)

# 출력 데이터 (= 라벨 정보) 원핫 인코딩
print('원핫 인코딩 전: ', Y_train[0])
Y_train = to_categorical(Y_train, 10)

print('원핫 인코딩 후: ', Y_train[0])
Y_test = to_categorical(Y_test, 10)

print('학습용 출력 데이터 모양: ', Y_train.shape)
print('평가용 출력 데이터 모양: ', Y_test.shape)
```  

>  
결과  
```
    원핫 인코딩 전:  9
    원핫 인코딩 후:  [0. 0. 0. 0. 0. 0. 0. 0. 0. 1.] # 10개니까
    학습용 출력 데이터 모양:  (60000, 10)
    평가용 출력 데이터 모양:  (10000, 10)
```  


임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
