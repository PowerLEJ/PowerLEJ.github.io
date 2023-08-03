---
layout: single
title:  "Mac os M1 Anaconda Install 맥 아나콘다 설치"
categories: anaconda
tag: anaconda
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Anaconda Install 아나콘다 설치  

```
brew install --cask anaconda
```  

### base 가상 환경의 자동 활성화 해제  
```
conda config --set auto_activate_base false
```  

### base 가상환경 deactive  
```
conda deactivate
```  

### Conda Command 콘다 명령어
<br />
Conda **Info** 콘다 정보 확인
```
conda info
```
<br />
Conda **Version** 콘다 버전 확인
```
conda --version # conda -V
```
<br />
Conda **Create** a virtual environment 가상 환경 생성
```
conda create -n 만드려는가상환경이름 설치하려는패키지버전(생략가능)
# ex) conda create -n test01 python=3.9.7
```
<br />
Conda **Delete** a virtual environment 가상 환경 삭제
```
conda env remove -n 삭제하려는가상환경이름 # conda remove -n 삭제하려는가상환경이름 --all
# ex) conda env remove -n test01
# ex) conda remove -n test01 --all
```
<br />
Conda **Env List** 가상 환경 리스트
```
conda env list # conda info --envs
```
<br />
Conda **Activate** 가상 환경 활성화
```
conda activate 만든가상환경이름
```
<br />
If Not Activated 만약 활성화가 안되면 Conda **Init**
```
conda init bash # 맨처음 터미널 열었을때 $가 뜨면
conda init zsh # 맨처음 터미널 열었을때 %가 뜨면 (나는 이거다)
```
<br />
Conda **Deactivate** 가상 환경 비활성화
```
conda deactivate 비활성화하려는가상환경이름
```
<br />
Conda **List** 설치된 패키지 리스트
```
conda list
```
<br />
Conda Package **List** 해당 패키지가 가상 환경에 있는지 확인
```
conda list 해당패키지명
# ex) conda list jupyter
```
<br />
Conda **Search** 설치된 패키지 버전 확인
```
conda search 알고싶은패키지명
# ex) conda search python
```
<br />
Conda Package **Install** 가상 환경에 패키지 설치
```
conda install 설치하려는패키지명
# ex) conda install python=3.11.4
# ex) conda install jupyter
```
<br />
Jupyter Notebook 실행  
```
jupyter notebook
```
<br />
[Conda Command Site (콘다 명령어) Click](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html){: .btn .btn--warning}