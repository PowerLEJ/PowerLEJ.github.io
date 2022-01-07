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

model = Sequential() # Keras Sequential 모듈 활용

input = X_train.shape[1] # (354, 12) 에서 1번째는 12이다 # < 입력층: 뉴런 12개 >

# Keras Dense 모듈 활용
# 은닉층 1 Dense는 은닉층에 대한 뉴런 200개랑 입력층의 12개 뉴런을 곱해서 2400개 그리고 은닉층 뉴런 200개 더해서 시냅스 총 2600개
model.add(Dense(200,
                input_dim=input,
                activation='relu')) # < 은닉층 1: 뉴런 200개, relu 활성화 >
# 은닉층 2추가
model.add(Dense(1000,
                activation='relu')) # <은닉층 2: 뉴런 1000개, relu 활성화 >
# 최종 출력층 추가
model.add(Dense(1)) # < 출력층: 뉴런 1개, 활성화 없음 >

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

## 평균 제곱 오차 이론  
- Mean Square Error (MSE)  
-- **MSE = (예측값 - 정답)제곱의 평균**  
-- 완벽한 예측의 경우 MSE는 0  
-- 오차가 커지면 MSE가 커짐  
-- 제곱오차들의 평균: 큰 오차들에 더 민감  

```python
######## 인공 신경망 학습 ########

# sgd : 함수의 기울기(경사)를 구하여 낮은 쪽으로 이동
# mse : (예측값 - 정답)제곱의 평균
# 최적화 함수와 손실 함수 지정 (인공신경망 학습 환경 설정)
# Keras compile 모듈 활용
model.compile(optimizer='sgd',
              loss='mse')

print('\nDNN 학습 시작')
begin = time()

# epochs 학습 시 몇 번 반복할지
# batch_size 한꺼번에 몇 개를 처리할지
# verbose 학습 내용의 결과를 매번 출력할지 0은 출력하지 않음, 1은 출력함
# Keras fit 모듈 활용 (인공신경망 학습)
model.fit(X_train,
          Y_train,
          epochs=MY_EPOCH,
          batch_size=MY_BATCH,
          verbose=1
          )
end = time()
print('총 학습 시간: {:.1f}초'.format(end-begin))
```  

>  
결과  
```
    DNN 학습 시작
    Epoch 1/500
    6/6 [==============================] - 0s 8ms/step - loss: 0.8163
    Epoch 2/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.5689
    Epoch 3/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.4617
    ~~~~~~~~~~~~~ 생 략 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Epoch 8/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.2358
    Epoch 9/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.2185
    Epoch 10/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.2043
    Epoch 11/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.1938
    Epoch 12/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.1861
    Epoch 13/500
    6/6 [==============================] - 0s 4ms/step - loss: 0.1773
    ~~~~~~~~~~~~~ 생 략 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Epoch 495/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.0420
    Epoch 496/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.0418
    Epoch 497/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.0417
    Epoch 498/500
    6/6 [==============================] - 0s 4ms/step - loss: 0.0414
    Epoch 499/500
    6/6 [==============================] - 0s 4ms/step - loss: 0.0416
    Epoch 500/500
    6/6 [==============================] - 0s 5ms/step - loss: 0.0413
    총 학습 시간: 15.0초
```  

## Evaluate 이론 / Predict 이론  
- Keras evaluate 함수  
-- 학습된 DNN을 **평가**할 때 사용  
-- 학습용 데이터 입력과 출력을 동시에 사용  
-- X_test와 Y_test 사용  

- Keras predict 함수  
-- 학습된 DNN으로 **추측**할 때 사용  
-- 새로운 데이터를 사용  
-- 새로운 데이터 부재 시 : 학습용 데이터 입력만 사용  
-- X_test 사용  

## Seaborn Regplot이론  
- 데이터 **시각화**에 많이 사용  
-- 산포도와 선형 모델을 동시에 구현  

```python
######## 인공 신경망 평가 및 활용 ########

# 신경망 평가 및 손실값 계산
# Keras evaluate 모듈 활용
loss = model.evaluate(X_test,
                      Y_test,
                      verbose=1)
# 152개 문제를 한 번만 풀었다
print('\nDNN 평균 제곱 오차 (MSE): {:.2f}'.format(loss))
```  

>  
결과  
```
    5/5 [==============================] - 0s 4ms/step - loss: 0.0958
    DNN 평균 제곱 오차 (MSE): 0.10
```

```python
# 신경망 활용 및 산포도 출력

# Keras predict 모듈 활용
pred = model.predict(X_test)

# Seaborn package를 이용하여 산포도 분석
sns.regplot(x=Y_test, y=pred) # x축은 정답, y축은 예측값

plt.xlabel("Actual Values")
plt.ylabel("Predicted Values")
plt.show()
```   

![tensor_class_01_03_02](/images/2022-01-07-tensorflow_class_01_03/tensor_class_01_03_02.png){: width="100%" height="100%"}{: .center}


임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
