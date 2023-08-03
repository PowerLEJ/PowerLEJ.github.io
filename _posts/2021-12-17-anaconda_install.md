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

### Homebrew Install 홈브루 설치

[Homebrew Site (홈브루 사이트) Click](https://brew.sh/index_ko){: .btn .btn--warning}

Homebrew **Install** 설치
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
<br />
Homebrew **Update** 업데이트
```
brew update
```
<br />
Homebrew **List** 설치된 패키지 리스트
```
brew list
```
<br />
Homebrew **Package Install** 패키지 설치
```
brew install 설치하려는패키지명
```
---

### Homebrew Command 홈브루 명령어
<br />
Homebrew **Package Search** 패키지 검색
```
brew search 검색하려는패키지명
```
<br />
Homebrew **Package Upgrade** 패키지 업그레이드
```
brew upgrade 업그레이드하려는패키지명
```
<br />
Homebrew **Package Uninstall** 패키지 삭제
```
brew uninstall 삭제하려는패키지명
```
<br />
Homebrew **Package Remove** 패키지 삭제
```
brew remove 삭제하려는패키지명
```
<br />
Homebrew **Package Cleanup** 이전 버전 패키지 삭제
```
brew cleanup 이전버전삭제하려는패키지명
```
<br />
Homebrew **Check** 검사하기
```
brew doctor
```
---

### Install Miniforge To Use Anaconda
<br />
아나콘다 설치를 위해 **miniforge** 설치
```
brew install miniforge
```

## Anaconda Use 아나콘다 사용

### Check if Anaconda is available
아나콘다 사용 가능 유무 확인
```
open -e .bash_profile
open -e .zshrc
```
---

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
# ex) conda install python=3.9.7
```
<br />
[Conda Command Site (콘다 명령어) Click](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html){: .btn .btn--warning}