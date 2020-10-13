---
title: "프로그래머스-정렬-가장 큰 수[정렬]"
date: 2020-10-13T15:52:46+09:00
draft: false

categories: ['Programmers']
tags: ['Programmers', 'Sorting', 'Algorithm']
author: "xfrnk2"
---
###### 프로그래머스-정렬-가장 큰 수[정렬][문제 바로가기](https://programmers.co.kr/learn/courses/30/lessons/42746)
---
### 나의 파이썬 풀이
~~~
def solution(numbers):
    numbers = map(str, numbers)
    answer = ''.join(sorted(numbers, reverse=True, key=lambda n: (n[0], n[1 % len(n)], n[2 % len(n)], n[3 % len(n)])))
    return answer if int(answer) else '0'
~~~
---
### Javascript 풀이 해석
> 친구 말로는 해당 문제의 블로그 등 온라인 상의 Javascript 풀이의 100% 아래와 같은 형태를 하고 있는데, sort 함수의 (b+a)-(a+b)가 이해가 어렵다고 하여 어떻게 설명할 수 있을지 알아보게 되었다.
  
~~~
function solution(numbers) {
    
    var answer = numbers.map(c=> c + '').
    				sort((a,b) => (b+a) - (a+b)).join('');
    
    return answer[0]==='0'? '0' : answer;
}
~~~
---
### 분석
###### [문법 웹문서 바로가기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

###### 문서 본문에서 발췌한 내용
  
![이미지1](https://user-images.githubusercontent.com/34790699/95827646-f4d38a80-0d6e-11eb-861f-391ffadae22b.png)
+ `compareFunction`이 제공되면, 이하 unordered list의 2번째, '모든 다른 요소에 대해 정렬합니다'를 보고 이해가 되었다.
+ 값이 양수이면 값을 뒤로 보내고 음수이면 제자리인 것으로 납득이 되었다.
+ 혹시나 하는 마음에 모든 다른 요소들에 대한 비교우위를 어떻게 가릴 수 있는지 값을 대입해서 증명해 보았다.
---
#### **대입 후 증명**  

###### [6, 10, 2]일 때.
**1. 6의 경우**
+ 6과 10 : 106-610 = -504로 음수
+ 6과 2 : 26-62 = -36으로 음수 

**2. 10의 경우**
+ 10과 2 : 210-102 = 108으로 음수
+ 10과 6 : 610-106 = 504로 양수

**3. 2의 경우**
+ 2와 6 : 62-26 = 36으로 양수
+ 2와 10 : 102-210 = 108으로 음수

###### 결론  
각각의 합계를 더해보면 6 = -540, 10 = 396, 2 = 72 이기 때문에,  
이것을 내림차순으로 정렬하면  6, 2, 10 순서이기 대문에 6210이 된다.

###### 또다른 예시
같은 방법으로 [3, 9, 12]도 해 보았다.
+ 3 기준 합계 : 54 - 189
+ 9 기준 합계 : -54 -783
+ 12 기준 합계 : 189 + 783
   
각각의 합계를 더해서 내림차순으로 정렬하면 9, 3, 12순서이기 때문에 9312가 된다.
