---
title: "피보나치 수열로 본 동적 프로그래밍(Dynamic Programming)"
date: 2021-07-07T15:55:01+09:00
draft: False

categories: ['Algorithm']
tags: ['Dynamic_programming']
author: "xfrnk2"
---
## **차례**
1. Memoization
2. Tabluation
3. 공간 최적화
---
## 1. Memoization
#### > 특징
+ 재귀함수를 사용한다.
+ 필요한 만큼만, 불필요한 경우는 제외하여 구한다. `필요한 계산을 요구, 필요하지 않은 경우는 요구하지 않응`.
+ 단, 최대 재귀호출 깊이에 제한을 받을 수 있음.
#### Code
~~~
def fib_memo(n, cache):

    if n < 3:
        return 1

    # 이미 n번째 피보나치를 계산했으면 저장된 값을 바로 리턴한다.
    if n in cache:
        return cache[n]

    cache[n] = fib_memo(n - 1, cache) + fib_memo(n - 2, cache)

    return cache[n]

def fib(n):
    # n번째 피보나치 수를 담는 사전
    fib_cache = {}

    return fib_memo(n, fib_cache)

# test
print(fib(10))
print(fib(50))
print(fib(100))
~~~
---
## 2. Tabluation
#### > 특징
+ 반복문을 사용한다.
+ 모든 경우의 수를 구한다. -> 시/공간복잡도 : `O(n)`
#### Code
~~~
def fib_tab(n):
    fib = [0 for _ in range(n+1)]
    fib[1], fib[2] = 1, 1
    for i in range(3, n+1):
        fib[i] += fib[i-2] + fib[i-1]
    return fib[n]


# test
print(fib_tab(10))
print(fib_tab(56))
print(fib_tab(132))
~~~
---
## 3. 공간 최적화
#### > 특징
+ `prev(1)`, `current(2)` 값 으로 `current, prev = prev + current, current`   
`n까지 반복` → 사용하는 메모리가 늘어나지 않으므로 공간복잡도 O(1)이니 효율적
#### Code
~~~
def fib_optimized(n):
    
    prev, current = 1, 1
    for _ in range(3, n + 1):
        current, prev = prev + current, current
    return current

# test
print(fib_optimized(16))
print(fib_optimized(53))
print(fib_optimized(213))
~~~
---