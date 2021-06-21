---
title: "Codility - lesson2_arrays_OddOccurrencesInArray"
date: 2021-06-22T01:39:12+09:00
draft: false

categories: ['Codility']
tags: ['Codility']
author: "xfrnk2"
---
### 문제 링크
https://app.codility.com/programmers/lessons/2-arrays/odd_occurrences_in_array/
### 풀이
코딜리티에서 처음 풀어본 문제
두번의 시도를 했는데, 각각 O(N^2)과  O(N) or O(N*log n)이 나왔다.
아래와 같이 reduce를 사용하니 시간복잡도가 개선되었다.
~~~
from functools import reduce

def solution(A):
    return reduce(lambda acc, cur : acc ^ cur, A, 0)
~~~
---
첫 번째로 작성했던 지저분한 코드
~~~
def solution(A):
    return eval('^'.join(list(map(str, A))))
~~~
