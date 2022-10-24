---
layout: single
title:  "Tensorflow Class 01 09 DNN-GAN"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## train_on_batch 이론  
- train_on_batch와 fit의 차이  
    - train_on_batch  
    -- 입력 데이터: batch 하나  
    -- 가중치 보정: 매 batch마다 한 번  
    -- 주 목적: batch 당 추가 정보 계산  
    -- epoch: 무의미  
    - fit  
    -- 입력 데이터: 학습용 데이터 전체  
    -- 매 batch마다 한 번  
    -- 모든 데이터로 전체 학습 진행  
    -- 중요한 파라미터  

- GAN에서 사용하는 학습법  
-- "Runs a single gradient update on a single batch of data"  
-- 한 batch로 학습하고, 한 번 가중치 보정  
-- 입력: (1)학습용 입력 데이터, (2)학습용 출력 데이터
-- 출력: 평균 손실값 (한 batch 당)  

- Numpy의 zeros 함수는 감별자에게 주어진 이미지가 가짜임을 판별토록 유도  
- Numpy의 ones 함수는 감별자에게 주어진 이미지가 진짜임을 판별토록 유도  

```python
################## 인공 신경망 학습 ####################

# 감별자 학습 방법
def train_discriminator():

    # 진짜 이미지 임의로 한 batch 추출
    total = X_train.shape[0]
    pick = np.random.randint(0, total, MY_BATCH)
    image = X_train[pick]

    # 숫자 1을 한 batch 생성
    all_1 = np.ones((MY_BATCH, 1))

    # 진짜 이미지로 감별자 한 번 학습
    d_loss_real = discriminator.train_on_batch(image,
                                              all_1)

    print(d_loss_real) # [ 손실값, 정확도 ]

    # 생성자를 이용하여 가짜 이미지 생성
    # 노이즈 벡터는 표준 정규 분포를 사용
    noise = np.random.normal(0, 1,
                             (MY_BATCH, MY_NOISE))
    fake = generator.predict(noise)
    print(fake.shape)

    # 숫자 0을 한 batch 생성
    all_0 = np.zeros((MY_BATCH, 1))
    print(all_0.shape)

    # 가짜 이미지로 감별자 한 번 학습
    d_loss_fake = discriminator.train_on_batch(fake,
                                               all_0)
    print(d_loss_fake)

    # 평균 손실과 정확도 계산
    d_loss = 0.5 * np.add(d_loss_real, d_loss_fake)
    print(d_loss)

    return d_loss

X_train = read_data()
discriminator, generator, gan = build_GAN()
train_discriminator()
```  

>  
결과  
```
    [2.0735437870025635, 0.0033333334140479565]
    (300, 28, 28, 1)
    (300, 1)
    [0.6490623354911804, 0.6899999976158142]
    [1.36130306 0.34666667]
```  

## 감별자 학습  
- 평균 0, 표준편차 1  
-- 진짜 이미지로 한 번 학습, 가짜 이미지로 학습, 총 2번 진행  
-- np.random.normal(0, 1)  
![tensor_class_01_09_01](/images/2022-01-11-tensorflow_class_01_09/tensor_class_01_09_01.png){: width="100%" height="100%"}{: .center}  

```python
# 생성자 학습 방법
def train_generator():

    # 노이즈 벡터는 표준 정규 분포를 사용
    noise = np.random.normal(0, 1,
                             (MY_BATCH, MY_NOISE))
    print(noise.shape)

    # 숫자 1을 한 batch 생성
    all_1 = np.ones((MY_BATCH, 1))
    print(all_1.shape)

    # 가짜 이미지로 생성자 한 번 학습
    g_loss = gan.train_on_batch(noise,
                                all_1)
    print(g_loss) # 손실값

    return g_loss

X_train = read_data()
discriminator, generator, gan = build_GAN()
train_generator()
```  

>  
결과  
```
    (300, 100)
    (300, 1)
    0.7370219826698303
```  

```python
# 샘플 이미지 N * N 출력
def sample(epoch):
    row = col = 4

    # 노이즈 벡터 생성
    noise = np.random.normal(0, 1,
                             (row * col, MY_NOISE))
    print(noise.shape)

    # 생성자를 이용하여 가짜 이미지 생성
    fake = generator.predict(noise)
    print(fake.shape)

    # 채널 정보 삭제
    fake = np.squeeze(fake)
    print(fake.shape)

    # 캔버스 만들기
    fig, spot = plt.subplots(row, col)

    # i행 j열에 가짜 이미지 추가
    cnt = 0
    for i in range(row):
        for j in range(col):
            spot[i, j].imshow(fake[cnt], cmap='gray')
            spot[i, j].axis('off')
            cnt += 1

    # plt.show()

    # 이미지를 PNG 파일로 저장
    path = os.path.join(MY_FOLDER,
                        'img-{}'.format(epoch))
    plt.savefig(path)
    plt.close()

X_train = read_data()
discriminator, generator, gan = build_GAN()
sample(0)
```  

>  
결과  
MY_FOLDER(output) 폴더 밑에 이미지가 img-0 파일명으로 저장된다
```
(16, 100)
(16, 28, 28, 1)
(16, 28, 28)
```  

```python
# GAN 학습
def train_GAN():
    begin = time()
    print('\nGAN 학습 시작')

    # MY_EPOCH = 200 # 5000번을 하면 너무 길어질까봐 200으로 설정함
    for epoch in range(MY_EPOCH + 1):
        d_loss = train_discriminator()
        g_loss = train_generator()

        # 매 50번 학습 때마다 결과와 샘플 이미지 생성
        if epoch % 50 == 0:
            print('에포크:', epoch,
                  '생성자 손실: {:.3f}'.format(g_loss),
                  '감별자 손실: {:.3f}'.format(d_loss[0]),
                  '감별자 정확도: {:.1f}%'.format(d_loss[1] * 100))
            sample(epoch)
    end = time()

    print('최종 학습 시간: {:.1f}초'.format(end - begin))


############# 컨트롤 타워 #############

# 데이터 준비
X_train = read_data()

# GAN 구현
discriminator, generator, gan = build_GAN()

# GAN 학습
train_GAN()
```  

>  
결과  
MY_FOLDER(output) 폴더 밑에 이미지가 img-0, 50, 100 등의 파일명으로 5000번 반복해서 50씩 이미지가 저장된다
```
    GAN 학습 시작
    에포크: 0 생성자 손실: 0.937 감별자 손실: 0.939 감별자 정확도: 52.0%
    에포크: 50 생성자 손실: 4.444 감별자 손실: 0.029 감별자 정확도: 100.0%
    에포크: 100 생성자 손실: 3.809 감별자 손실: 0.033 감별자 정확도: 100.0%
    에포크: 150 생성자 손실: 2.943 감별자 손실: 0.036 감별자 정확도: 100.0%
    에포크: 200 생성자 손실: 1.935 감별자 손실: 0.109 감별자 정확도: 100.0%
    에포크: 250 생성자 손실: 3.105 감별자 손실: 0.032 감별자 정확도: 100.0%
    에포크: 300 생성자 손실: 4.000 감별자 손실: 0.014 감별자 정확도: 100.0%
    에포크: 350 생성자 손실: 3.387 감별자 손실: 0.030 감별자 정확도: 100.0%
    에포크: 400 생성자 손실: 3.518 감별자 손실: 0.025 감별자 정확도: 100.0%
    에포크: 450 생성자 손실: 3.947 감별자 손실: 0.024 감별자 정확도: 100.0%
    ~~~~~~~~~~~~~ 생 략 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    에포크: 4600 생성자 손실: 4.523 감별자 손실: 0.179 감별자 정확도: 94.2%
    에포크: 4650 생성자 손실: 3.546 감별자 손실: 0.280 감별자 정확도: 89.7%
    에포크: 4700 생성자 손실: 3.863 감별자 손실: 0.204 감별자 정확도: 92.8%
    에포크: 4750 생성자 손실: 4.028 감별자 손실: 0.304 감별자 정확도: 88.3%
    에포크: 4800 생성자 손실: 4.132 감별자 손실: 0.201 감별자 정확도: 91.7%
    에포크: 4850 생성자 손실: 4.254 감별자 손실: 0.276 감별자 정확도: 89.3%
    에포크: 4900 생성자 손실: 4.731 감별자 손실: 0.199 감별자 정확도: 92.7%
    에포크: 4950 생성자 손실: 4.426 감별자 손실: 0.227 감별자 정확도: 91.3%
    에포크: 5000 생성자 손실: 5.094 감별자 손실: 0.140 감별자 정확도: 95.2%
    최종 학습 시간: 349.9초
```  

![tensor_class_01_09_02](/images/2022-01-11-tensorflow_class_01_09/tensor_class_01_09_02.png){: width="100%" height="100%"}{: .center}  




임성규 교수의 TensorFlow로 배우는 머신러닝 알고리즘 강의를 들으며 정리하였습니다.  
또한 이곳저곳에서 구글링하며 공부하였습니다.  
