---
title: "정렬 알고리즘 - 선택 정렬(Selection Sort)"
date: 2020-07-11T22:55:01+09:00
draft: True

categories: ['TIL']
tags: ['TIL']
author: "xfrnk2"
---
*선택정렬의 원리와 특징 및 성능에 대해서 정리하는 글이다.*

> **<학습 목표>**  
> * 선택정렬의 원리를 이해한다.  
> * 선택정렬의 특징을 파악한다.  
> * 선택정렬의 성능을 기억한다.  
> * 모든 과정을 설명할 수 있다.   
> * 선택정렬을 프로그래밍 언어로 구현할 수 있다.
---
### **선택정렬의 원리**

+ 비교 연산을 통해 주어진 배열의 최솟값을 탐색
+ 배열의 가장 앞에있는 원소와 최솟값을 교환
+ 나머지 부분을 가지고 위의 과정을 반복 
---
### **선택정렬의 정렬 과정**
   
   
배열의 크기 = n = 5,   
최솟값의 인덱스 = least_index
~~~
[5, 3, 4, 2, 1] 
~~~
---
    
+ 0번째와 1~4번째값을 비교하여 보다 작은 값의 index를 least_index에 저장한다.  
+ 결과 : 최솟값 = 1, least_index = 4  
+ least_index 위치의 값과 0번째 값을 swap한다.
  
**결과 : 0번째 값(1) 정렬**
~~~
[1, 3, 4, 2, 5] 
~~~
    
---
  
+ 1번째와 2~4번째 값을 비교하여 보다 작은 것의 index를 least_index에 저장한다.  
+ 최솟값 = 2, least_index = 3  
+ least_index 위치의 값과 1번째 값을 swap한다. 
   
**결과 : 0, 1번째 값(1, 2) 정렬**   
~~~
[1, 2, 4, 3, 5] 
~~~
---
    
+ 2번째와 3~4번째 값을 비교하여 보다 작은 것의 index를 least_index에 저장한다.  
+ 최솟값 = 3, least_index = 3  
+ least_index 위치의 값과 2번째 값을 swap한다. 
      
**결과 : 0, 1, 2번째 값(1, 2, 3) 정렬**
~~~
[1, 2, 3, 4, 5] 
~~~
---
   
+ 3번째와 4번째 값을 비교하여 보다 작은 것의 index를 least_index에 저장한다.  
+ 최솟값 = 4, least_index = 3  
+ least_index 위치의 값과 3번째 값을 swap한다. (사실상 제자리)
      
**결과 : 0, 1, 2, 3번째 값(1, 2, 3, 4) 정렬**
~~~
[1, 2, 3, 4, 5] 
~~~
  

---
### **Source Code**
  
~~~
n = len(array) 

for i in range(0, n-1):
	least_index = i
	for j in range(i+1, n):
		if arr[j] < arr[least_index]:
			least_index = j

	arr[i], arr[least_index] = arr[least_index], arr[i]
~~~
  
---
  
### **선택정렬의 성능**
작성중