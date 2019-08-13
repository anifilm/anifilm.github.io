---
layout: post
title: "파이썬 Conda 가상환경 만들기"
tags: [python, miniconda, conda, envs]
comments: true
---

#### 미니콘다 파이썬 콘다 가상환경 관련 명령


가상환경 추가
conda create --name [가상환경이름]


가상환경 활성
source activate [가상환경이름]


가상환경 비활성
conda deactivate


가상환경 목록 확인
conda env list


가상환경 삭제
conda remove --name [가상환경이름] --all