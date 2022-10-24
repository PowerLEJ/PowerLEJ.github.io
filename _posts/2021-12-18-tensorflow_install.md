---
layout: single
title:  "Mac os M1 monterey Tensorflow Install 맥 텐서플로우 설치"
categories: tensorflow
tag: tensorflow
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Tensorflow Install 텐서플로우 설치

### Anaconda activate
<br />
Conda **Activate** 가상 환경 활성화
```
conda activate 만든가상환경이름
```
---

### Tensorflow Install

[tensorflow-metal PluggableDevice (텐서플로우) Click](https://developer.apple.com/metal/tensorflow-plugin/){: .btn .btn--warning}

텐서플로우 설치
```
conda install -c conda-forge openblas
conda install -c apple tensorflow-deps
pip install tensorflow-macos
pip install tensorflow-metal
pip install tensorflow-macos --no-dependencies
pip install flatbuffers --no-dependencies
```
<br />

{% include video id="_gtJ0ntv8Yc" provider="youtube" %}
[GPU Training (참고 자료) Click](https://github.com/Razin-Tailor/tensorflow-metal-test/blob/main/test.ipynb){: .btn .btn--warning}

---

### Jupyter Notebook
<br />
주피터 노트북 사용
```
conda install jupyter
jupyter --version
jupyter notebook # 실행
```
<br />
jupyter-lab 을 실행하고 싶은데
```
AttributeError: 'ExtensionManager' object has no attribute '_extensions'
```
이런 에러가 나온다.
<br />
다운그레이드 했다.
```
pip install jupyter_server==1.6.4
```