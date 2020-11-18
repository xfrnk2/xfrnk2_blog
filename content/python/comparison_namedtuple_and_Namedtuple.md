---
title: "Python - collections의 namedtuple과 typing의 NamedTuple 비교"
date: 2020-11-18T13:10:57+09:00
draft: false

categories: ['Python']
tags: ['Python', 'immutable']
author: "xfrnk2"
---
## 코드 사용 예 - typing의 NamedTuple
~~~
from typing import NamedTuple

class Employee(NamedTuple):
    name: str
    id: int
~~~
---
## 코드 사용 예 - collections의 namedtuple
~~~
from collections import namedtuple

Employee = namedtuple('Employee', ['name', 'id'])
~~~
---
> typing의 Namedtuple을 사용하는 편이 보다 자연스러운 인터페이스를 선언할 수 있다. collections의 namedtuple의 신버전이 typing의 NamedTuple이다.

## typinng의 NamedTuple을 사용하면 좋은 점
+ type 이름을 두 번 반복 할 필요가 없다. ( 예시에서의 "Employee")
+ type을 사용자가 직접 정의 할 수 있다. (예 : 독 스트링 또는 일부 메소드 추가) -> NamedTuple의 하위 클래스가 아님
+ 상속이 가능하다.
+ 기본값 지정 가능(그러나 Python 3.7부터 collections의 namedtuple에도 default 키워드가 추가되었으므로 더이상 이점은 아니다)
---
## NamedTuple을 짧게 사용하는 사용예시
~~~
PageDimensions = NamedTuple("PageDimensions", [('width', int), ('height', int)])
~~~
---
## Python3.7부터 추가된 collections.namedtuple의 _field_defaults (기본값) 속성 용법  
  
1. 기존 사용방법과 같이 코드 작성
  
~~~
Account = namedtuple('Account', 'name num logincount')
~~~
  
2. 뒤에 콤마(,) 후 default=[]괄호 안에 숫자를 입력  
  
~~~
Account = namedtuple('Account', 'name num logincount', defaults=[1])
~~~
  
> 가장 뒤에 선언된 속성부터 기본값이 매겨지게 되므로, 이후 Accounts._field_defaults를 출력하면 {'logincount' : 1}이 된다. 
  
> 이와 다르게 default=[1, 2]라고 입력한 뒤 출력하면 {'num' : 1, 'logincount' : 2}의 결과가 나온다.
  
> 만약 default=[1, 2, 3]을 입력한 뒤 출력하면 {'name' : 1, 'num' : 2, 'logincount' : 3}의 결과가 나온다.
  
  
+ 참고링크  
https://docs.python.org/3/library/collections.html  
https://pythontic.com/containers/namedtuple/_field_defaults  
  
+ typing의 NamedTuple과 collections의 namedtuple의 차이 비교  
https://stackoverflow.com/questions/50766461/whats-the-difference-between-namedtuple-and-namedtuple