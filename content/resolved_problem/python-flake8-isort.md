---
title: "Python - isort Error"
date: 2021-04-05T11:16:34+09:00
draft: false

categories: ['resolved problem']
tags: ['resolved problem']
author: "xfrnk2"
---
### 문제 발생 상황
- PEP8을 따르도록 수정하기 위해 command prompt에 `flake8 file.py`을 입력했다.
- isort error가 표시되었다.
---
### 문제 해결
+ https://pypi.org/project/isort/
+ isort는 python에서 import하는 라이브러리들을 자동으로 정렬해준다.
```
isort file.py
```
+ 이후 나오는 질문에선 y를 입력하면 된다.
