---
title: "Python - 많은 양의 데이터를 효율적으로 입력받는 방법"
date: 2021-06-02T02:19:34+09:00
draft: false

categories: ['Python']
tags: ['Python']
author: "xfrnk2"
---
### sys.stdin.readline()
많은 데이터 입력을 받고자 할 때는 sys.stdin.readline()을 사용하는 것이 유리하다.  
(자바에서 Scanner보다 BufferedReader가 빠른것과 같은 맥락에서)
~~~
import sys
raw_input = sys.stdin.readline
N = int(raw_input())
~~~
  
+ 덧붙여서, 위 코드에서 raw_input()은 함수를 담은 변수의 이름에 불과한데, Python 3부터는 raw_input이 없어지고 이것이 input()으로 이름이 바뀌었다.