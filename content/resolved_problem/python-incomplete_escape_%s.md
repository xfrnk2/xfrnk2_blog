---
title: "Python - source.error(“incomplete escape %s” % escape, len(escape))"
date: 2021-04-05T11:17:34+09:00
draft: false

categories: ['resolved problem']
tags: ['resolved problem']
author: "xfrnk2"
---

### 문제 발생 상황
~~~
raise source.error("incomplete escape %s" % escape, len(escape))
re.error: incomplete escape \U at position 72 (line 4, column 3)
~~~
+ PEP8을 따르도록 터미널에서 flake8을 입력하니 위와 같은 에러가 발생했다.
+ 검색 결과 보통의 경우 위의 메세지는 파일 어딘가에 `\U` 문자열이 포함되어 있을때 발생 하는 것으로 나타났지만 `\U`는 없을 뿐더러 **전혀** 다른 경우였다.
---
### 에러 발생 원인
+ 함수의 입력값으로서 파라미터를 Typing할 때. 추상메서드를 사용한 빈 객체 파라미터에 대한 타입을 명시할 때 발생하는 것이 확인되었다.
---
### 문제점이 있던 코드
+ event.py
~~~
from abc import ABC

class Event(ABC):
    pass
~~~
---
+ puyo.py
~~~
 def reflect_event(self, event: Event):	  <- 이 곳이다.
        if isinstance(event, VoidEvent):
            return self.position
'''
	이하 생략...
'''
~~~
---
### 해결 방법
+ puyo.py의 첫번째 줄, `def reflect_event(self, event: Event):`의 Type hint인 가장 뒷부분의 `event: Event` 를 뒤의 Event를 지우고, `event`로 수정했더니 에러가 더이상 발생하지 않았다.

+ 내가 올렸었던 Stackoverflow에 같은 내용의 질문에 대한 링크 -> [**이동**]((https://stackoverflow.com/questions/66675461/python-3-8-source-errorincomplete-escape-s-escape-lenescape?noredirect=1#comment117864310_66675461))