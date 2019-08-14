---
layout: post
title: "파이썬 Conda 가상환경 만들기"
tags: [python, conda, envs]
comments: true
---

파이썬 Conda 가상환경 관련 명령어
<br>
<br>
#### 가상환경 추가
```bash
$ conda create --name `[가상환경이름]`
```
<br>
<br>
#### 가상환경 활성
```bash
$ source activate `[가상환경이름]`
```
<br>
<br>
#### 가상환경 비활성
```bash
$ conda deactivate
```
<br>
<br>
#### 가상환경 목록 확인
```bash
$ conda env list
```
<br>
<br>
#### 가상환경 삭제
```bash
$ conda remove --name `[가상환경이름]` --all
```