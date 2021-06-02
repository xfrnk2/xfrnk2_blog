---
title: "Python - 딕셔너리 원소의 갯수를 보다 효율적으로 counting하는 방법"
date: 2021-06-02T02:20:34+09:00
draft: false

categories: ['Python']
tags: ['Python']
author: "xfrnk2"
---
### Dictionary의 get 메소드를 활용하자.
~~~
counts = dict()
for i in items:
  counts[i] = counts.get(i, 0) + 1
~~~
+ 시간복잡도적 측면에서 collections의 Counter보다 더 낫다.
출처 : [링크](https://stackoverflow.com/questions/3496518/using-a-dictionary-to-count-the-items-in-a-list)
