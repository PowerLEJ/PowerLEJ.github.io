---
layout: single
title:  "Tensorflow Basics Study 01"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Convert Jupyter to Markdown
터미널에서 변환하려는 파일명이 있는 폴더로 이동한 다음
```
jupyter nbconvert --to markdown 변환하려는파일명.ipynb
```

## Reference Site
참고 사이트

[Tensorflow Site (텐서플로우 사이트) Click](https://www.tensorflow.org/api_docs/python/tf){: .btn .btn--warning}

[Tensorflow Study Youtube (텐서플로우 공부 유튜브) Click](https://www.youtube.com/watch?v=HPjBY1H-U4U&list=PLhhyoLH6IjfxVOdVC1P1L5z5azs0XjMsb&index=3){: .btn .btn--danger}

## Study

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
import tensorflow as tf
print(tf.__version__)

#physical_devices = tf.config.list_physical_devices('GPU') # 물리적 장치 tf.config.list_physical_devices((옵션 문자열) 이 장치 유형과 일치하는 장치만 포함합니다. 예: "CPU" 또는 "GPU")
#tf.config.experimental.set_memory_growth(physical_devices[0], True) # tf.config.experimental.set_memory_growth(PhysicalDevice 구성, (Boolean) 메모리 증가를 활성화 또는 비활성화할지 여부)
```
>
결과
```
    2.7.0
```    

```python
# Initialization of Tensors
x = tf.constant(4, shape=(1, 1), dtype=tf.float32) # tf.constant(value, dtype=None, shape=None, name='Const')
print(x)
x = tf.constant([[1, 2, 3], [4, 5, 6]])
print(x)

x = tf.ones((3, 3)) # tf.ones(shape, dtype=tf.dtypes.float32, name=None)
print(x)
x = tf.zeros((2, 3)) # tf.zeros(shape, dtype=tf.dtypes.float32, name=None)
print(x)
x = tf.eye(3) # tf.eye(num_rows, num_columns=None, batch_shape=None, dtype=tf.dtypes.float32, name=None)
print(x)
x = tf.random.normal((3, 3), mean=0, stddev=1) # tf.random.normal(shape, mean=0.0, stddev=1.0, dtype=tf.dtypes.float32, seed=None, name=None)
print(x)
x = tf.random.uniform((1, 3), minval=0, maxval=1) # tf.random.uniform(shape, minval=0, maxval=None, dtype=tf.dtypes.float32, seed=None, name=None)
print(x)
x = tf.range(9) # tf.range(limit, delta=1, dtype=None, name='range')
print(x)
x = tf.range(start=1, limit=10, delta=2) # tf.range(start, limit, delta=1, dtype=None, name='range')
print(x)
x = tf.cast(x, dtype=tf.float64) # tf.cast(x, dtype, name=None)
print(x)

# tf.float (16, 32, 64), tf.int (8, 16, 32, 64), tf.bool
```
>
결과
```
    tf.Tensor([[4.]], shape=(1, 1), dtype=float32)
    tf.Tensor(
    [[1 2 3]
     [4 5 6]], shape=(2, 3), dtype=int32)
    tf.Tensor(
    [[1. 1. 1.]
     [1. 1. 1.]
     [1. 1. 1.]], shape=(3, 3), dtype=float32)
    tf.Tensor(
    [[0. 0. 0.]
     [0. 0. 0.]], shape=(2, 3), dtype=float32)
    tf.Tensor(
    [[1. 0. 0.]
     [0. 1. 0.]
     [0. 0. 1.]], shape=(3, 3), dtype=float32)
    tf.Tensor(
    [[ 0.39888087  0.09034941  1.2782422 ]
     [ 0.4332027  -1.3594952  -0.51380026]
     [-0.91104424  0.1791235  -1.227694  ]], shape=(3, 3), dtype=float32)
    tf.Tensor([[0.31015325 0.7175082  0.8377377 ]], shape=(1, 3), dtype=float32)
    tf.Tensor([0 1 2 3 4 5 6 7 8], shape=(9,), dtype=int32)
    tf.Tensor([1 3 5 7 9], shape=(5,), dtype=int32)
    tf.Tensor([1. 3. 5. 7. 9.], shape=(5,), dtype=float64)
```    

```python
# Mathematical Operations
x = tf.constant([1, 2, 3])
print(x)
y = tf.constant([9, 8, 7])
print(y)

z = tf.add(x, y)
print(z)
z = x + y
print(z)

z = tf.subtract(x, y)
print(z)
z = x - y
print(z)

z = tf.divide(x, y)
print(z)
z = x / y
print(z)

z = tf.multiply(x, y)
print(z)
z = x * y
print(z)

z = tf.tensordot(x, y, axes=1)
print(z)
z = tf.reduce_sum(x * y, axis=0)
print(z)

z = x ** 5 # 제곱
print(z)
```
>
결과
```
    tf.Tensor([1 2 3], shape=(3,), dtype=int32)
    tf.Tensor([9 8 7], shape=(3,), dtype=int32)
    tf.Tensor([10 10 10], shape=(3,), dtype=int32)
    tf.Tensor([10 10 10], shape=(3,), dtype=int32)
    tf.Tensor([-8 -6 -4], shape=(3,), dtype=int32)
    tf.Tensor([-8 -6 -4], shape=(3,), dtype=int32)
    tf.Tensor([0.11111111 0.25       0.42857143], shape=(3,), dtype=float64)
    tf.Tensor([0.11111111 0.25       0.42857143], shape=(3,), dtype=float64)
    tf.Tensor([ 9 16 21], shape=(3,), dtype=int32)
    tf.Tensor([ 9 16 21], shape=(3,), dtype=int32)
    tf.Tensor(46, shape=(), dtype=int32)
    tf.Tensor(46, shape=(), dtype=int32)
    tf.Tensor([  1  32 243], shape=(3,), dtype=int32)
```    

```python
x = tf.random.normal((2, 3))
print(x)
y = tf.random.normal((3, 4))
print(y)
z = tf.matmul(x, y) # z = x @ y
print(z)

x = tf.constant([[1, 2], [3, 4]])
print(x)
y = tf.constant([[10], [20]])
print(y)
z = tf.matmul(x, y) # z = x @ y
print(z)

x = tf.constant([[1, 2, 3], [4, 5, 6]])
print(x)
y = tf.constant([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
print(y)
z = tf.matmul(x, y)
print(z)
z = tf.matmul(x, y) # z = x @ y
print(z)
```

>
결과
```
    tf.Tensor(
    [[ 1.1922456   0.58976084  0.5359963 ]
     [ 0.17715207  0.36108223 -1.7451191 ]], shape=(2, 3), dtype=float32)
    tf.Tensor(
    [[-0.0143341   1.8737693  -0.3539741  -0.9299258 ]
     [ 0.12946281 -0.40897357 -0.9996165   0.8854193 ]
     [-0.12436708  0.42386383 -1.781893   -0.1898017 ]], shape=(3, 4), dtype=float32)
    tf.Tensor(
    [[-0.00739796  2.2199862  -1.9666469  -0.6882473 ]
     [ 0.26124278 -0.55542386  2.6859646   0.48619747]], shape=(2, 4), dtype=float32)
    tf.Tensor(
    [[1 2]
     [3 4]], shape=(2, 2), dtype=int32)
    tf.Tensor(
    [[10]
     [20]], shape=(2, 1), dtype=int32)
    tf.Tensor(
    [[ 50]
     [110]], shape=(2, 1), dtype=int32)
    tf.Tensor(
    [[1 2 3]
     [4 5 6]], shape=(2, 3), dtype=int32)
    tf.Tensor(
    [[ 1  2  3  4]
     [ 5  6  7  8]
     [ 9 10 11 12]], shape=(3, 4), dtype=int32)
    tf.Tensor(
    [[ 38  44  50  56]
     [ 83  98 113 128]], shape=(2, 4), dtype=int32)
    tf.Tensor(
    [[ 38  44  50  56]
     [ 83  98 113 128]], shape=(2, 4), dtype=int32)
```    

```python
# Indexing
x = tf.constant([0, 1, 1, 2, 3, 1, 2, 3])
# print(x[:]) # 전체
# print(x[1:]) # 1부터 끝까지
# print(x[1:3]) # 1부터 3미만
# print(x[::2]) # 2의 배수 빼고 스킵
# print(x[::-1]) # 거꾸로

indices = tf.constant([0, 3])
x_ind = tf.gather(x, indices) # x에 있는 것 중에서 0번째와 3번째에 있는 것 가져와라 tf.gather(params, indices, validate_indices=None, axis=None, batch_dims=0, name=None)
print(x_ind)

x = tf.constant([[1, 2],
                [3, 4],
                [5, 6]])
print(x[0,:])
print(x[0:1,:])
print(x[0:2,:])
```
>
결과
```
    tf.Tensor([0 2], shape=(2,), dtype=int32)
    tf.Tensor([1 2], shape=(2,), dtype=int32)
    tf.Tensor([[1 2]], shape=(1, 2), dtype=int32)
    tf.Tensor(
    [[1 2]
     [3 4]], shape=(2, 2), dtype=int32)
```    

```python
# Reshaping
x = tf.range(9)
print(x)

x = tf.reshape(x, (3, 3)) # tf.reshape(tensor, shape, name=None)
print(x)

x = tf.transpose(x, perm=[1, 0]) # tf.transpose(a, perm=None, conjugate=False, name='transpose')
print(x)

x = tf.constant([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
x = tf.transpose(x, perm=[0, 2, 1])
print(x)
```

>
결과
```
    tf.Tensor([0 1 2 3 4 5 6 7 8], shape=(9,), dtype=int32)
    tf.Tensor(
    [[0 1 2]
     [3 4 5]
     [6 7 8]], shape=(3, 3), dtype=int32)
    tf.Tensor(
    [[0 3 6]
     [1 4 7]
     [2 5 8]], shape=(3, 3), dtype=int32)
    tf.Tensor(
    [[[ 1  4]
      [ 2  5]
      [ 3  6]]
     [[ 7 10]
      [ 8 11]
      [ 9 12]]], shape=(2, 3, 2), dtype=int32)
```    
