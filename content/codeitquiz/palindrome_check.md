---
title: "알고리즘 - 팰린드롬 문자열 판별하기(파이썬)"
date: 2021-06-24T01:13:19+09:00
draft: false

categories: ['Algorithm']
tags: ['Algorithm']
author: "xfrnk2"
---
> 수강중인 강의로부터 수행한 선수 과제의 일부이다.   
> 예전에 팰린드롬을 판별하는 문제를 가지고 고전했던 기억이 떠올라서 별도의 기록으로 남기면 좋겠다는 생각으로 작성하였다.

~~~
def is_palindrome(word):
    pivot = len(word)//2
    
    for i in range(1, pivot+1):
        if word[pivot-i] != word[pivot+i]:
            return False
    
    return True
            

# 테스트
print(is_palindrome("racecar"))
print(is_palindrome("stars"))
print(is_palindrome("토마토"))
print(is_palindrome("kayak"))
print(is_palindrome("hello"))
~~~
+ 정렬을 직접 구현해보는 연습을 통해 pivot이라는 개념을 사용하는데 익숙해졌나보다.
+ 비교적 쉬운 편에 속했다.