---
layout: single
title:  "Tensorflow Class 01 02"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## 인공 신경망과 DNN 이론
### 심층 신경망 : Deep Neural Network (DNN)
- 인공 신경망(ANN)은 인간 두뇌의 뉴런과 시냅스를 이산수학의 "그래프"에서 시작함  
- 심층 신경망 (DNN)은 ANN 중 은닉층의 갯수가 많아 복잡한 비선형적 관계를 설명하는 데 효과적임  

## 상자 그림 & 산포도 이론
- 상자 그림(Box Plot) 이론 : 데이터 특징을 잘 요약해 주는 5가지 값을 시각적으로 표현함  
- 산포도 (Scatter Plot) 이론 : 직각 좌표계에서 두 변수에 대응하는 관측값을 점들로 표현한 그림임  

## 패키지 설치
- pandas, matplotlib, seaborn, keras, scikit-learn

```python
# 파이썬 패키지 가져오기
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from time import time

from keras.models import Sequential
from keras.layers import Dense
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

# 하이퍼 파라미터
MY_EPOCH = 500 # 학습 시 몇 번 반복할지
MY_BATCH = 64 # 한꺼번에 몇 개를 처리할지
```


```python
############ 데이터 준비 #############

# 데이터 파일 읽기
# 결과는 pandas의 데이터 프레임 형식
heading = ['CRIM', 'ZN', 'INDUS', 'CHAS', 'NOX', 'RM', 'AGE', 'DIS', 'RAD', 'TAX', 'PTRATIO', 'LSTAT', 'MEDV']

raw = pd.read_csv('housing.csv') # 엑셀 파일

# 데이터 원본 출력
print('원본 데이터 샘플 10개')
print(raw.head(10))

print('원본 데이터 통계')
print(raw.describe())
```

>
결과
```
    원본 데이터 샘플 10개
        CRIM    ZN  INDUS  CHAS    NOX  ...  RAD  TAX  PTRATIO  LSTAT  MEDV
    0  0.00632  18.0   2.31     0  0.538  ...    1  296     15.3   4.98  24.0
    1  0.02731   0.0   7.07     0  0.469  ...    2  242     17.8   9.14  21.6
    2  0.02729   0.0   7.07     0  0.469  ...    2  242     17.8   4.03  34.7
    3  0.03237   0.0   2.18     0  0.458  ...    3  222     18.7   2.94  33.4
    4  0.06905   0.0   2.18     0  0.458  ...    3  222     18.7   5.33  36.2
    5  0.02985   0.0   2.18     0  0.458  ...    3  222     18.7   5.21  28.7
    6  0.08829  12.5   7.87     0  0.524  ...    5  311     15.2  12.43  22.9
    7  0.14455  12.5   7.87     0  0.524  ...    5  311     15.2  19.15  27.1
    8  0.21124  12.5   7.87     0  0.524  ...    5  311     15.2  29.93  16.5
    9  0.17004  12.5   7.87     0  0.524  ...    5  311     15.2  17.10  18.9  
    [10 rows x 13 columns]
    원본 데이터 통계
                CRIM          ZN       INDUS  ...     PTRATIO       LSTAT        MEDV
    count  506.000000  506.000000  506.000000  ...  506.000000  506.000000  506.000000
    mean     3.613524   11.363636   11.136779  ...   18.455534   12.653063   22.532806
    std      8.601545   23.322453    6.860353  ...    2.164946    7.141062    9.197104
    min      0.006320    0.000000    0.460000  ...   12.600000    1.730000    5.000000
    25%      0.082045    0.000000    5.190000  ...   17.400000    6.950000   17.025000
    50%      0.256510    0.000000    9.690000  ...   19.050000   11.360000   21.200000
    75%      3.677083   12.500000   18.100000  ...   20.200000   16.955000   25.000000
    max     88.976200  100.000000   27.740000  ...   22.000000   37.970000   50.000000  
    [8 rows x 13 columns]
```    

## Z-점수 정규화

- 머신러닝 알고리즘  
-- 데이터의 특정 패턴을 찾음  
-- 새로운 값 예측  

- 넓은 범위 요소의 영향  
-- 기계학습을 할 때 범위가 더 넓은 요소가 큰 영향을 미침  
-- 범위가 작은 요소는 의미가 거의 묻혀 버림  
--> 따라서 제트점수 정규화를 진행한다  

- Pandas 데이터 프레임  
-- 2차원 테이블에 특화된 구조  
-- Numpy 행렬과 호환됨

- **Z-점수 = (X - 평균) / 표준편차**  
-- 평균은 0, 표준편차는 1이 되게 함  
-- Fit transform 함수로 Z-점수 정규화 진행  
-- Pandas의 head와 describe 함수로 데이터 내용 확인함  

- 예시  
-- A는 AS시험 1800점 / 2400점  
-- B는 BS시험 24점 / 36점    

-- AS 평균은 1500, 표준편차는 300  
-- BS 평균은 21, 표준편차는 5    

-- Z-점수는 A는 1, B는 0.6  
--> 따라서 A가 더 잘함  

```python
# Z-점수 정규화
# 결과는 numpy의 n-차원 행렬 형식
scaler = StandardScaler()
z_data = scaler.fit_transform(raw)

# numpy에서 pandas로 전환 (numpy와 pandas 데이터 형식이 달라서)
# header 정보 복구 필요
z_data = pd.DataFrame(z_data, columns=heading)

# 정규화 된 데이터 출력
print('정규화 된 데이터 샘플 10개')
print(z_data.head(10))

print('정규화 된 데이터 통계')
print(z_data.describe())
```  

>  
결과  
```
    정규화 된 데이터 샘플 10개
        CRIM        ZN     INDUS  ...   PTRATIO     LSTAT      MEDV
    0 -0.419782  0.284830 -1.287909  ... -1.459000 -1.075562  0.159686
    1 -0.417339 -0.487722 -0.593381  ... -0.303094 -0.492439 -0.101524
    2 -0.417342 -0.487722 -0.593381  ... -0.303094 -1.208727  1.324247
    3 -0.416750 -0.487722 -1.306878  ...  0.113032 -1.361517  1.182758
    4 -0.412482 -0.487722 -1.306878  ...  0.113032 -1.026501  1.487503
    5 -0.417044 -0.487722 -1.306878  ...  0.113032 -1.043322  0.671222
    6 -0.410243  0.048772 -0.476654  ... -1.505237 -0.031268  0.039964
    7 -0.403696  0.048772 -0.476654  ... -1.505237  0.910700  0.497082
    8 -0.395935  0.048772 -0.476654  ... -1.505237  2.421774 -0.656595
    9 -0.400729  0.048772 -0.476654  ... -1.505237  0.623344 -0.395385  
    [10 rows x 13 columns]
    정규화 된 데이터 통계
                CRIM            ZN  ...         LSTAT          MEDV
    count  5.060000e+02  5.060000e+02  ...  5.060000e+02  5.060000e+02
    mean  -8.513173e-17  3.306534e-16  ... -1.595123e-16 -4.247810e-16
    std    1.000990e+00  1.000990e+00  ...  1.000990e+00  1.000990e+00
    min   -4.197819e-01 -4.877224e-01  ... -1.531127e+00 -1.908226e+00
    25%   -4.109696e-01 -4.877224e-01  ... -7.994200e-01 -5.994557e-01
    50%   -3.906665e-01 -4.877224e-01  ... -1.812536e-01 -1.450593e-01
    75%    7.396560e-03  4.877224e-02  ...  6.030188e-01  2.685231e-01
    max    9.933931e+00  3.804234e+00  ...  3.548771e+00  2.989460e+00  
    [8 rows x 13 columns]
```  

## 지도 학습과 사분할 이론  
- Pandas 패키지를 사용해서 Boston Dataset을 2분할 함  
- Scikit-learn 패키지를 사용해서 Boston Dataset을 4분할 함  
학습용 입력값 X_train / 학습용 출력값 Y_train  
평가용 입력값 X_test  / 평가용 출력값 Y_test  

```python
# 데이터를 입력과 출력으로 분리
print('\n분리 전 데이터 모양: ', z_data.shape)
X_data = z_data.drop('MEDV', axis=1) # axis=1 컬럼을 누락, axis=0 행을 누락
Y_data = z_data['MEDV']

# 데이터를 학습용과 평가용으로 분리
# X_train, X_test, Y_train, Y_test 순서 중요
# test_size는 평가용 데이터를 전체 데이터에서 무작위로 몇 퍼센트 무시할 건지 지정
X_train, X_test, Y_train, Y_test = train_test_split(X_data, Y_data, test_size=0.3)

print('\n학습용 입력 데이터 모양', X_train.shape)
print('학습용 출력 데이터 모양', Y_train.shape)
print('평가용 입력 데이터 모양', X_test.shape)
print('평가용 출력 데이터 모양', Y_test.shape)
```   

>  
결과  
```
    분리 전 데이터 모양:  (506, 13) # (506, 13) 506개의 데이터(엑셀에서 집값 데이터), 각각의 데이터가 13개로 이루어져 있다
    -----------
    학습용 입력 데이터 모양 (354, 12) # 506의 70% / 13개 요소 중에 1개 drop 했으니까 12개, 2차원 데이터 모양
    학습용 출력 데이터 모양 (354,) # 1차원 데이터 모양
    평가용 입력 데이터 모양 (152, 12)
    평가용 출력 데이터 모양 (152,)
```  

## Bosteon 데이터 상자 그림  
- Seaborn boxplot 함수를 사용해서 상자그림 구현    

```python
# 상자그림 출력
sns.set(font_scale=2)
sns.boxplot(data=z_data, palette='dark')
plt.show()
```   

![tensor_class_01_02_01](/images/2022-01-06-tensorflow_class_01_02/tensor_class_01_02_01.png){: width="100%" height="100%"}{: .center}




임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
