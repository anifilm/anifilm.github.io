---
layout: post
title: "파이썬 개발 가상환경(VirtualEnv)"
tags: [python, venv]
comments: true
---

#### 파이썬 가상환경 관련 명령어
<br>
가상환경 추가 (폴더 생성)
```bash
$ python3 -m venv [가상환경이름]
```
<br>
가상환경 활성 (가상환경 폴더 'test-env')
```bash
$ source test-env/bin/activate
```
[윈도우 환경]
```bash
test-env\Script\activate.bat
```
<br>
가상환경 비활성
```bash
$ deactivate
```
[윈도우 환경]
```bash
test-env\Script\deactivate.bat
```
<br>
자료 출처: https://docs.python.org/ko/3/tutorial/venv.html