---
layout: single
title:  "Tensorflow Class 01 07 RNN"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 자동 이미지 캡션 기술  
- CNN을 통해 인식 후 언어(RNN) 완성함  

## 순환 신경망 이론  
- David Rumelhart (1986)  
-- 시계열 데이터를 학습하는 딥러닝 기술  
-- 기준 시점(t)과 다음 시점(t+1)에 네트워크를 연결  
-- AI 번역, 음성 인식, 주가 예측의 대표적 기술  

- RNN 마스터 구조 사용  
-- 마스터 구조를 입력 데이터의 숫자 만큼 풀어 헤침 (unfold)  

![tensor_class_01_07_01](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_01.png){: width="100%" height="100%"}{: .center}  

## LSTM 이론  
- vanishing gradient  
-- 기울기 유실 문제 
-- 옛날 일을 기억 못함  
-- 오래 전의 데이터에 의한 기울기 값이 소실되는 문제  

- Hochreiter & Schmidhuber (1997)  
-- RNN의 기울기 유실 문제 해결  
-- 2가지 종류의 정보를 다음 단에 전달  

### LSTM cell state 개념  
- 하나의 컨베이어 벨트처럼 전체 체인을 통과하는 연결 라인 구축함  
-- 현 단계 정보를 수정하여 계속 다음 단계에 전달  
-- 게이트(gate)를 활용해서 정보를 더하거나 제거  
-- 셀스테이트 라인 자체에 활성화 함수 없음 -> 베네싱 그래이언 문제에서 해방됨  
-- 제재나 가감 없으면 정확도가 떨어질 수 있음  

### LSTM gating 이론  
- 게이트(gate)를 활용해서 Cell state 라인에 정보를 추가/삭제 양을 조절하는 기능 수행  
    - 망각 Gate: 전 단계에서 오는 정보를 얼마나 잊어 버릴까 결정. [0, 1]  
    - 새 정보 Gate: 새로 들어오는 입력값을 얼마나 받아 들일까 결정. [-1, 1]  
    - Cell State 출력 Gate: 다음 단에 전해질 cell state의 양을 결정  
    -- 셀스테이트 출력 위한 활성화 함수 사용 안함  
    -> 망각과 새정보 게이트를 어떻게 합성할 것인지 결정하는 가중치가 존재하지 않음  
    - 현재 단 출력 Gate: 현재 단의 출력을 결정  

## 패키지 설치
- pandas, numpy, matplotlib, seaborn, keras, scikit-learn

```python
# 파이썬 패키지 가져오기
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from time import time
from sklearn.preprocessing import MinMaxScaler

from keras.models import Sequential
from keras.layers import InputLayer, Dense
from keras.layers import LSTM

# 하이퍼 파라미터
MY_PAST = 12 # 과거 몇 달치 사용할 건지
MY_SPLIT = 0.8 # 얼마만큼을 학습용으로 사용할지
MY_UNIT = 300 # LSTM셀 안에 내부 구조의 차원수
MY_SHAPE = (MY_PAST, 1) # 입력으로 들어갈 모양 (2차원)

MY_EPOCH = 300 # 반복 학습수
MY_BATCH = 64 # 병렬 계산 데이터수
np.set_printoptions(precision=3) # numpy내용 출력 시 소수점 3자리까지

############### 데이터 준비 #################

# 데이터 파일 읽기
# 결과는 pandas의 데이터 프레임 형식
raw = pd.read_csv('airline.csv',
                  header=None,
                  usecols=[1])

# 시계열 데이터 시각화
plt.plot(raw)
plt.show()
```   
![tensor_class_01_07_02](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_02.png){: width="100%" height="100%"}{: .center}  

```python
# 데이터 원본 출력
print('원본 데이터 샘플 13개')
print(raw.head(13))

print('\n원본 데이터 통계')
print(raw.describe())
```  

>  
결과  
```
    원본 데이터 샘플 13개
        1
    0   112
    1   118
    2   132
    3   129
    4   121
    5   135
    6   148
    7   148
    8   136
    9   119
    10  104
    11  118
    12  115
                        -
    원본 데이터 통계
                    1
    count  144.000000
    mean   280.298611
    std    119.966317
    min    104.000000
    25%    180.000000
    50%    265.500000
    75%    360.500000
    max    622.000000
```  

## MinMax 정규화 이론  
- [0, 1] 스케일링: 모든 데이터값을 0, 1 범위로 전환함  
- 로베스트 스케일러: 아웃라이어에 약한 민맥스의 단점 개선함  
- 노멀라이저: 데이터의 크기보다 방향이 중요한 경우에 사용함  

```python
# MinMax 데이터 정규화
scaler = MinMaxScaler()
s_data = scaler.fit_transform(raw) # raw는 데이터프레임인데 numpy 형식으로 전환

print('\nMinMax 정규화 형식', type(s_data))

# 정규화 데이터 출력
df = pd.DataFrame(s_data) # s_data를 데이터프레임으로 역전환

print('\n정규화 데이터 샘플 13개')
print(df.head(13))

print('\n정규화 데이터 통계')
print(df.describe())
```  

>  
결과  
```
    MinMax 정규화 형식 <class 'numpy.ndarray'>
                                               -
    정규화 데이터 샘플 13개
            0
    0   0.015444
    1   0.027027
    2   0.054054
    3   0.048263
    4   0.032819
    5   0.059846
    6   0.084942
    7   0.084942
    8   0.061776
    9   0.028958
    10  0.000000
    11  0.027027
    12  0.021236
                                                -
    정규화 데이터 통계
                    0
    count  144.000000
    mean     0.340345
    std      0.231595
    min      0.000000
    25%      0.146718
    50%      0.311776
    75%      0.495174
    max      1.000000
```  

## 데이터 분할 묶음 이론  
- Air Passenger Dataset: 144개 데이터  
-- 12달치는 입력(12달치 여행자 수), 13번째는 출력(13번째 달 여행자 수  예측)  
-- 데이터 분할을 할 필요가 있는데 두 가지 옵션이 있다  

![tensor_class_01_07_03](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_03.png){: width="100%" height="100%"}{: .center}  

- 파이썬 리스트를 사용할 것이다  
-- <span style="color:red">3: 입력수</span>, <span style="color:blue">12: 데이터수</span>  
-- 총 묶음수: <span style="color:blue">12</span>-<span style="color:red">3</span>+2 = 9  
-- 마지막 9번째 묶음: 8, 9, 10, 11  
-- For-loop 범위: 0에서 <span style="color:blue">12</span>-<span style="color:red">3</span> (8까지만 방문)  
-- 분할 범위: i에서 i+<span style="color:red">3</span>+1 (i+3까지만 처리)  

![tensor_class_01_07_04](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_04.png){: width="100%" height="100%"}{: .center}  

```python
# 13개 묶음으로 데이터 분할
# 결과는 python 리스트
bundle = []
for i in range(len(s_data) - MY_PAST):
    bundle.append(s_data[i: i+MY_PAST+1])

# 데이터 분할 결과 확인
print('\n총 13개 묶음의 수:', len(bundle))
print(bundle[0])
print(bundle[1])

# numpy로 전환
print('분할 데이터의 타입:', type(bundle))
bundle = np.array(bundle)
print('분할 데이터의 모양:', bundle.shape)
```  

>  
결과  
```
    총 13개 묶음의 수: 132
    [[0.015]
    [0.027]
    [0.054]
    [0.048]
    [0.033]
    [0.06 ]
    [0.085]
    [0.085]
    [0.062]
    [0.029]
    [0.   ]
    [0.027]
    [0.021]]
    [[0.027]
    [0.054]
    [0.048]
    [0.033]
    [0.06 ]
    [0.085]
    [0.085]
    [0.062]
    [0.029]
    [0.   ]
    [0.027]
    [0.021]
    [0.042]]
    분할 데이터의 타입: <class 'list'>
    분할 데이터의 모양: (132, 13, 1)
```  

```python
# 데이터를 입력과 출력으로 분할
X_data = bundle[:, 0:MY_PAST] # 행은 처음부터 끝까지 즉 :, 열은 처음부터 11까지 즉 0:12
Y_data = bundle[:, -1] # 행은 처음부터 끝까지 즉 :, 열은 맨 마지막 즉 -1

# 데이터를 학습용과 평가용으로 분할
split = int(len(bundle) * MY_SPLIT) # 몇 프로를 학습용으로 할지
# 105개(13개 묶음)는 학습용, 27개는 평가용
X_train = X_data[: split]
X_test = X_data[split:]

Y_train = Y_data[: split]
Y_test = Y_data[split:]

# 최종 데이터 모양
print('\n학습용 입력 데이터 모양:', X_train.shape)
print('학습용 출력 데이터 모양:', Y_train.shape)

print('평가용 입력 데이터 모양:', X_test.shape)
print('평가용 출력 데이터 모양:', Y_test.shape)
```  

>  
결과  
```
    학습용 입력 데이터 모양: (105, 12, 1)
    학습용 출력 데이터 모양: (105, 1)
    평가용 입력 데이터 모양: (27, 12, 1)
    평가용 출력 데이터 모양: (27, 1)
```  


```python
################# 인공 신경망 구현 ################

# RNN 구현
# 케라스 RNN은 2차원 입력만 허용
model = Sequential()
model.add(InputLayer(input_shape=MY_SHAPE))
model.add(LSTM(MY_UNIT))

model.add(Dense(1,
                activation='sigmoid'))

print('\nRNN 요약')
model.summary()
```  
>  
결과  
```
    RNN 요약
    Model: "sequential"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    lstm (LSTM)                 (None, 300)               362400    
    dense (Dense)               (None, 1)                 301       
    =================================================================
    Total params: 362,701
    Trainable params: 362,701
    Non-trainable params: 0
    _________________________________________________________________
```  

## Sigmoid 활성화 함수  
- 딥러닝에서 가장 많이 쓰이는 활성화 함수  
-- Step function을 대체: 전 구간에서 미분 가능  
-- 장점: 미분 계산이 간단  
s'(x) = s(x){1 - s(x)}  
-- 단점: 기울기 유실 문제  

![tensor_class_01_07_05](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_05.png){: width="100%" height="100%"}{: .center}  

## RMSprop 최적화 알고리즘 이론  
- Root Mean Square Propagation  
-- 제프리 힌턴 교수(역전파 알고리즘(백 프로페게이션 알고리즘))팀 제안
-- 학습률 상황에 맞게 조절(시냅스의 가중치를 보정할 때 정하는 숫자)  
-- 진동을 줄이는 효과  

![tensor_class_01_07_06](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_06.png){: width="100%" height="100%"}{: .center}  


```python
################# 인공 신경망 학습 ###############

# 최적화 함수와 손실 함수 지정
model.compile(optimizer='rmsprop',
              loss='mse') # 정확한 값을 들고 오지 않아도 된다

begin = time()
print('\nRNN 학습 시작')

model.fit(X_train,
          Y_train,
          epochs=MY_EPOCH,
          batch_size=MY_BATCH,
          verbose=1)

end = time()
print('총 학습 시간: {:.1f}초'.format(end - begin))
```

>  
결과  
```
    RNN 학습 시작
    Epoch 1/300
    2/2 [==============================] - 4s 131ms/step - loss: 0.0829
    Epoch 2/300
    2/2 [==============================] - 0s 22ms/step - loss: 0.0710
    Epoch 3/300
    2/2 [==============================] - 0s 16ms/step - loss: 0.0488
    Epoch 4/300
    2/2 [==============================] - 0s 14ms/step - loss: 0.0777
    Epoch 5/300
    2/2 [==============================] - 0s 13ms/step - loss: 0.0372
    Epoch 6/300
    2/2 [==============================] - 0s 14ms/step - loss: 0.0246
    Epoch 7/300
    2/2 [==============================] - 0s 15ms/step - loss: 0.0199
    Epoch 8/300
    2/2 [==============================] - 0s 15ms/step - loss: 0.0062
    ~~~~~~~~~~~~~ 생 략 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Epoch 294/300
    2/2 [==============================] - 0s 13ms/step - loss: 0.0022
    Epoch 295/300
    2/2 [==============================] - 0s 14ms/step - loss: 0.0036
    Epoch 296/300
    2/2 [==============================] - 0s 14ms/step - loss: 0.0019
    Epoch 297/300
    2/2 [==============================] - 0s 16ms/step - loss: 0.0016
    Epoch 298/300
    2/2 [==============================] - 0s 17ms/step - loss: 0.0018
    Epoch 299/300
    2/2 [==============================] - 0s 19ms/step - loss: 0.0027
    Epoch 300/300
    2/2 [==============================] - 0s 14ms/step - loss: 0.0018
    총 학습 시간: 14.6초
```  

```python
################## 인공 신경망 평가 #################

# RNN 평가
loss = model.evaluate(X_test,
                      Y_test,
                      verbose=1)

print('최종 MSE 손실값: {:.3f}'.format(loss))
```  

>  
결과  
```
    1/1 [==============================] - 0s 275ms/step - loss: 0.0161
    최종 MSE 손실값: 0.016
```  

## Inverse Transformation(역전환) 이론  
- MinMax 정규화 공식  
Xscaled = (X - min(X)) / (max(X) - min(X))  
- MinMax 역전환 공식  
X = Xscaled(max(X) - min(X)) + min(X)  
-- LSTM은 [0, 1] 정규화 데이터로 학습  
-- LSTM predict 결과는 [0, 1] 정규화됨  
-- Scikit-learn의 inverse_transform 함수 사용  
-- [0, 1] 역전환으로 원래 여행자 수 복귀  

- LSTM 평가와 관련된 데이터 모양  
-- X_test(평가용 입력 데이터): numpy 3차원 행렬  
-- Y_test(평가용 출력 데이터): numpy 2차원 행렬  
-- Pred(LSTM 예측값): numpy 2차원 행렬  

- LSTM 예측 결과의 시각화  
-- seaborn package의 lineplot를 사용해보려고 한다  
-- 근데 Lineplot 함수는 1차원 행렬 요구  

- Numpy flatten 함수  
-- Numpy로 n-차원 행렬을 1차원으로 축소 가능  

```python
# RNN 추측
pred = model.predict(X_test)
pred = scaler.inverse_transform(pred)
pred = pred.flatten().astype(int)
print('\n추측 결과 원본: ', pred)

# 정답 역전환
truth = scaler.inverse_transform(Y_test)
truth = truth.flatten().astype(int)
print('\n정답 원본: ', truth)
```  

>  
결과  
```
추측 결과 원본:  [400 371 358 338 328 340 359 399 458 498 503 454 409 375 366 359 355 371
 385 424 470 510 518 486 450 421 404]
정답 원본:  [359 310 337 360 342 406 396 420 472 548 559 463 407 362 405 417 391 419
 461 472 535 622 606 508 461 390 432]
```  

## Seaborn Lineplot 함수  
- 선형 그래프 생성  
-- LSTM 정확도 평가: MSE 손실값 숫자 하나로는 부족  
-- LSTM의 예측값과 dataset의 정답을 시각적으로 비교  

```python
# line plot 구성
axes = plt.gca()
axes.set_ylim([0, 650]) # y축의 최소값과 최대값의 범위 지정(0부터 650까지)

sns.lineplot(data=pred, label='pred', color='blue')
sns.lineplot(data=truth, label='truth', color='red')

plt.show()
```  

![tensor_class_01_07_07](/images/2022-01-09-tensorflow_class_01_07/tensor_class_01_07_07.png){: width="100%" height="100%"}{: .center}  




임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
