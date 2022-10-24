---
layout: single
title:  "Tensorflow Class 01 04 CNN"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## CNN  
- CNN 기술의 필요성  
-- CNN은 이미지 처리에 큰 걸림돌인 큰 화소 처리 문제를 해결하기 위해 개발  
- CNN 기술의 목적  
-- 이미지에서 핵심 특징을 추출하며 화소를 줄여감  
-- 합성곱: 필터를 사용해서 이미지에서 핵심 특징 추출  
-- 화소가 많은 이미지를 빨리 처리하면서 정확도 유지  
-- 합성곱, ReLU 활성화, 풀링을 반복 적용해서 학습  

## 합성곱(Convolution) 이론  
- 학습 필터와 이미지 데이터를 합성하여 곱하는 원리  
- Convolution에서 padding과 stride는 결과물의 화소 수를 결정하는 역할  

- 필터로 이미지 처리: filter  
-- 학습 과정을 거치면서 숫자 보정이 이루어짐  
-- 이미지가 CNN을 거쳐 예측값이 Error라면 역전파 알고리즘 경사하강법 사용  

![tensor_class_01_04_01](/images/2022-01-08-tensorflow_class_01_04/tensor_class_01_04_01.png){: width="50%" height="50%"}{: .center}

- 이미지 화소수의 변화를 결정: padding  
- 특징 추출이 주된 목적  
-- 패딩 옵션 : same(입력된 이미지, 출력된 이미지 크기가 같음) / valid(패딩이 없는 이미지로 합성곱 진행)  

![tensor_class_01_04_02](/images/2022-01-08-tensorflow_class_01_04/tensor_class_01_04_02.png){: width="100%" height="100%"}{: .center}

- 이미지에 필터를 적용하는 간격 지정 : Stride(보폭)  
-- 필터를 이동시킬 때 몇 칸을 건너뛰는지(오른쪽과 아래로) 정함  
-- stride가 클 수록 이미지의 크기가 빠른 속도로 줄어듦  
-- 출력 이미지의 크기를 축소함  
-- 동시에 특징 추출도 병행함  

### 합성곱 채널 이론  
- 예: 입력 channel = 3, 출력 channel = 1  
-- 입력 채널의 수와 상관 없이, 출력 채널의 숫자만큼 필터가 사용된다  
-- 필터 개수는 1개  
-- 입력 채널 수와 같은 두께의 3차원 필터 사용  
-- RGB 칼라 이미지를 얻어내는 방식  

>  
```
(8*8 입력 이미지 3개) *합성곱 (4*4*3 필터 1개) = (5*5 출력 이미지 1개)
```  

![tensor_class_01_04_03](/images/2022-01-08-tensorflow_class_01_04/tensor_class_01_04_03.png){: width="100%" height="100%"}{: .center}  

- 예: 입력 channel = 3, 출력 channel = 2  
-- 필터 개수는 2개  

>  
```
(8*8 입력 이미지 3개) *합성곱 (4*4*3 필터 1개) = (5*5 출력 이미지 1개)  -> 출력 채널 1  
(8*8 입력 이미지 3개) *합성곱 (4*4*3 필터 1개) = (5*5 출력 이미지 1개)  -> 출력 채널 2  
```  

## 풀링 이론  
- Convolutional layer 바로 다음에 위치  
-- 화소 축소: 채널의 크기는 고정  
-- 오버피팅(Overfitting) 발생하는 것을 방지  
-- 필터는 없고, 창(window) 이용  
-- 처리 속도 빠르고, 별도의 학습 과정 없이 단순히 이미지의 크기를 줄임  
-- 과접합을 방지하기 위해 사용  

![tensor_class_01_04_04](/images/2022-01-08-tensorflow_class_01_04/tensor_class_01_04_04.png){: width="50%" height="50%"}{: .center}   

## LeNet-5  
- 최초 CNN인 LeNet-5는 convolution과 pooling이 두 번 반복되는 구조를 가짐  

## MNIST 패션 데이터  
-- 10가지 종류의 패션 아이템을 담은 7만 개의 데이터  
-- 28*28 화소의 흑백 이미지  

임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
