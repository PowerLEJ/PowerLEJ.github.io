---
layout: single
title:  "Tensorflow Class 01 06 CNN"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## CNN 설계도  
![tensor_class_01_06_01](/images/2022-01-09-tensorflow_class_01_06/tensor_class_01_06_01.png){: width="100%" height="100%"}{: .center}  


```python
############## 인공 신경망 구현 ###############

# CNN 구현 (순차적 방법)
model = Sequential()

# 입력층
model.add(InputLayer(input_shape=(28, 28, 1)))

# 첫 번째 합성곱 출력
model.add(Conv2D(32, # 채널
                 kernel_size=2, # 필터크기
                 padding='same',
                 activation='relu'))

model.add(MaxPool2D(pool_size=2))

# 두 번째 합성곱 블럭
model.add(Conv2D(64,
                 kernel_size=2,
                 padding='same',
                 activation='relu'))

model.add(MaxPool2D(pool_size=2))

# fully-connected 층으로 마무리
model.add(Flatten()) # CNN에서 합성곱 처리된 화소들을 뉴런화함

model.add(Dense(128,
                activation='relu'))

model.add(Dense(10,
                activation='softmax'))

print('\nCNN 요약')
model.summary()
```  

>  
결과  
```
    CNN 요약
    Model: "sequential"
    _________________________________________________________________
    Layer (type)                Output Shape              Param #   
    =================================================================
    conv2d (Conv2D)                (None, 28, 28, 32)      160       
    max_pooling2d (MaxPooling2D)   (None, 14, 14, 32)      0         
    conv2d_1 (Conv2D)              (None, 14, 14, 64)      8256      
    max_pooling2d_1 (MaxPooling2D) (None, 7, 7, 64)        0                                   
    flatten (Flatten)              (None, 3136)            0         
    dense (Dense)                  (None, 128)             401536    
    dense_1 (Dense)                (None, 10)              1290      
    =================================================================
    Total params: 411,242
    Trainable params: 411,242
    Non-trainable params: 0
    _________________________________________________________________
```  

## Softmax 활성화 함수  
- [0, 1] 정규화의 한 종류  
-- 자연지수, 함수(e의 y승 대입)  
-- 출력값들의 총합은 항상 1: 확률과 동일  
-- 분류 문제 해결에 많이 사용  

![tensor_class_01_06_02](/images/2022-01-09-tensorflow_class_01_06/tensor_class_01_06_02.png){: width="80%" height="80%"}{: .center}  

## Adam 최적화 함수 이론  
- Adaptive moment estimation  
-- RMSProp, Momentum을 동시에 사용함  

![tensor_class_01_06_03](/images/2022-01-09-tensorflow_class_01_06/tensor_class_01_06_03.png){: width="80%" height="80%"}{: .center}  

## Cross Entropy 손실 함수  
- 예측값 중 정답 위치 번째만 log를 취함  
-- 원핫 인코딩과 softmax를 함께 사용  
-- 정답 경우: 예측값이 1이면 손실은 0  
-- 오답 경우: 예측값이 1보다 적으면 손실값은 커짐  

![tensor_class_01_06_04](/images/2022-01-09-tensorflow_class_01_06/tensor_class_01_06_04.png){: width="80%" height="80%"}{: .center}  

```python
############# 인공 신경망 학습 ##############

# 최적화 함수와 손실 함수 지정
# Adam: 경사하강법을 개선한 최적화 알고리즘
# loss=crossentropy: binary는 둘 중에 하나를 분류할 때, categorical는 두가지 이상의 것을 분류할 때
# loss=mse 를 써도 되지만 categorical_crossentropy 이게 더 좋음
# metrics['acc']: 정확도를 출력하고 싶을 때
model.compile(optimizer='adam',
              loss='categorical_crossentropy',
              metrics=['acc'])

begin = time()
print('\nCNN 학습 시작')

model.fit(X_train,
          Y_train,
          epochs=MY_EPOCH, # 몇 번 반복
          batch_size=MY_BATCH, # 몇 개씩 계산할지
          verbose=1) # 학습 내용 결과 출력할지1 안할지0
end = time()
print('총 학습 시간: {:.1f}초'.format(end - begin))
```

>  
결과  
```
    CNN 학습 시작
    Epoch 1/3
    200/200 [==============================] - 8s 37ms/step - loss: 0.5899 - acc: 0.7924
    Epoch 2/3
    200/200 [==============================] - 7s 37ms/step - loss: 0.3621 - acc: 0.8708
    Epoch 3/3
    200/200 [==============================] - 7s 37ms/step - loss: 0.3169 - acc: 0.8862
    총 학습 시간: 22.6초
```  

```python
################# 인공 신경망 평가 ################
# CNN 평가
_, score = model.evaluate(X_test,
                          Y_test,
                          verbose=1)

print('최종 정확도: {:.2f}%'.format(score * 100))
```  

>  
결과  
```
    313/313 [==============================] - 2s 7ms/step - loss: 0.3140 - acc: 0.8862
    최종 정확도: 88.62%
```  

## 혼동 행렬과 F1 점수 이론  
![tensor_class_01_06_05](/images/2022-01-09-tensorflow_class_01_06/tensor_class_01_06_05.png){: width="80%" height="80%"}{: .center}  

- 혼동 행렬 (Confusion Matrix)  
--  분류 문제의 정확도를 계산할 적에 가장 많이 사용하는 방법  
-- 정확도를 한 눈에 볼 수 있도록 함  

- F1 점수 이론  
-- 조화평균:  
n개의 양수에 대해 그 역수들을 산술평균한 것의 역수  
평균을 구하는 모든 숫자들의 중요도가 비슷하도록 해줌  

```python
# CNN 추측
pred = model.predict(X_test) # predict은 10000개의 이미지를 cnn에 넣으면 10개의 softmax된 확률로 계산
pred = np.argmax(pred, axis=1) # numpy의 argmax는 2차원에서 1차원으로 줄여줌
truth = np.argmax(Y_test, axis=1) # 정답 truth : Y_test는 10000,10인데 axis로 1차원으로 줄여줌

# 혼동 행렬
print('\n혼동 행렬')
print(confusion_matrix(truth, pred))

# F1 점수
f1 = f1_score(truth, pred, average='micro')
print("\nF1 점수: {:.3f}".format(f1))
```  

>  
결과  
```
    혼동 행렬
    [[813   0  12  36   8   4 116   0  11   0]
    [  2 969   0  21   4   0   2   0   2   0]
    [ 17   1 765   9 115   0  90   0   3   0]
    [ 15   4   6 906  28   0  35   0   6   0]
    [  1   2  31  29 866   0  70   0   1   0]
    [  0   0   0   1   0 955   0  27   1  16]
    [122   2  45  30  96   0 689   0  16   0]
    [  0   0   0   0   0  10   0 956   1  33]
    [  0   1   1   3   2   1   3   4 984   1]
    [  0   0   0   0   0   3   0  37   1 959]]
    F1 점수: 0.886
```


임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
