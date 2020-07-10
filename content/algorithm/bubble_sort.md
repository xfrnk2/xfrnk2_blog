---
title: "정렬 알고리즘 - 버블 정렬(Bubble Sort) 정리"
date: 2020-07-10T00:08:44+09:00
draft: False

categories: ['Algorithm']
tags: ['Sorting']
author: "xfrnk2"
---
*버블정렬의 원리와 특징 및 성능에 대해서 정리하는 글이다.*

> **학습 목표**  
> 버블정렬의 원리와 특징 및 성능에 대해서 설명할 수 있다.  
> 버블정렬을 프로그래밍 언어로 구현할 수 있다.
---
### 버블정렬의 원리

+ 모든 인접한 값의 비교를 위해 왼쪽 또는 오른쪽에서 시작하여 반대편 끝으로 이동
+ 인접한 두 값을 비교하여 왼쪽 값이 더 큰 경우 자리를 바꾸는 과정을 반복
---
### 버블정렬의 정렬 과정
> + 가장 오른쪽 값(최댓값)부터 왼쪽 방향(최솟값 방향)으로 정렬되는 과정을 설명  
> + 비교연산의 기준이 되는 시작하는 위치는 가장 끝에서 가장 끝 바로 하나 전의 위치까지, 진행 방향으로 1씩 이동  

  
**초기 배열**
~~~
[4, 5, 3, 2]
~~~
배열의 크기는 n, n = 4이다.
   
--- 
  
**1. 첫번째 원소부터 n-1번째 원소까지 순회하며 오른쪽 원소와 비교를 수행한다**
  
   
   
+ 1, 2번째 값(4와 5) 비교시에는 교환이 일어나지 않는다
  
~~~
[4, 5, 3, 2]
~~~
  
+ 2, 3번째 값을 비교 후 교환(5와 3)
~~~
[4, 3, 5, 2]
~~~
+ 3, 4번째 값을 비교 후 교환(5와 2)
~~~
[4, 3, 2, 5]
~~~
> 가장 오른쪽 원소인 최댓값 5가 정렬된 것을 확인할 수 있다.
---
  
**2. 첫번째 원소부터 n-2번째 원소까지 순회하며 오른쪽 원소와 비교를 수행한다**
  


~~~
[4, 3, 2, 5]
~~~  


+ 1, 2번째 값을 비교 후 교환(4와 3) 

~~~
[3, 4, 2, 5]
~~~  


+ 2, 3번째 값을 비교 후 교환(4와 2)

~~~
[3, 2, 4, 5]
~~~  
  
> 최댓값보다 하나 작은 원소까지(4,5)가 정렬된 것을 확인할 수 있다.
---
    
**3. 첫번째 원소부터 n-3번째 원소까지 순회하며 오른쪽 원소와 비교를 수행한다**
  
  
~~~
[3, 2, 4, 5]
~~~
   
+ 1, 2번째 값을 비교 후 교환(3와 2)
  
  
~~~
[2, 3, 4, 5]
~~~
   
> 첫번째 원소를 제외하고 모든 원소가 정렬을 마쳤다. 정렬 완료.
  
---
### 버블정렬의 Python 코드
+ 위의 정렬과정을 Python으로 옮긴 코드이다.
  
~~~
array = [4, 5, 3, 2]
n = len(array)  # array의 길이(크기)

for i in range(n - 1, 0, -1):
    for j in range(i):
        if array[j] > array[j+1]:
            array[j], array[j+1] = array[j+1], array[j]
print(array)
~~~
  

### 버블정렬의 성능

+ 배열의 크기가 n일때 n - 1 번, n - 2번, n - 3번의 비교가 일어나는 것을 확인할 수 있다.
+ $n-1, n-2, ..., 2, 1 = n(n-1)/2$ 이므로, 시간복잡도는 $O(n^2)$이다.
+ 비교 횟수는 최선, 평균, 최악의 경우 모두 일정하며 성능 또한 동일하다.
  
### 버블정렬의 특징
+ 비교 기반의 정렬이다.
+ 안정적 정렬이다.
> 안정적 정렬이란, 동일한 값을 갖는 데이터가 여러 개 있을 때 정렬 전의 상대적인 순서가 정렬 후에도 그대로 유지됨을 의미한다. 안정적이지 않은 정렬의 경우, 동일한 값의 두 데이터가 서로 교환되는 경우가 발생하지만 버블정렬은 교환이 발생하지 않는다.
+ 제자리 정렬이다.
> 제자리 정렬은 정렬되는 항목 외에 $O(1)$개의 메모리만 필요하며 $O(log(n))$개의 추가적인 메모리를 인플레이스로 간주한다. 버블 정렬은 주어진 공간 외에 추가적인 공간을 사용하지 않는 정렬이다.

*ps. 공부한 것을 스스로 작성하였습니다.  
만약 이상한 점이 있거나, 의견이 있으시다면 코멘트 해주세요!*