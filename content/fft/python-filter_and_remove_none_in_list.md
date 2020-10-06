---
title: "Python - 특이점이 온 filter함수, list 안의 빈 값(None)을 제거하기"
date: 2020-10-06T18:58:11+09:00
draft: false

categories: ['Food For Thought']
tags: ['Python', 'FFT']
author: "xfrnk2"
---
## filter 함수의 특이점 발견 동기
> 레스토랑 시뮬레이션 코딩 중 작성한 코드의 일부이다.   
>(좋은 코드인지는 모르겠다.)
~~~
cooks = [cook if not cook.update() else finished_order.append(cook.order_info) for cook in self.__cooks]
self.__cooks = list(filter(None, cooks))
~~~
> + `else`를 연산하는 과정에서 `None`이 원소로 들어가는 것을 확인,  
> None 원소들을 제거해주기 위한 방법을 검색해봤다.  
> + *`list(filter(None, array))`*의 형태로 쓰란다.  
> + 그래서 형태를 모방한 코드를 작성했는데.... 그런데..  
> 왜 이렇게 되지? 싶은, 크게 눈에 띈 부분이 있었다.


---
## 특이점

**`filter(적용시킬 함수, 적용시킬 요소들)`의 형태로**  
**사용을 해왔지만,**  
**함수 자리에 None을 넣으니 적용되는 형태가 그 반대가 되는 것으로 보였다.**  
**예를 들면 원소 x % 2 == 0을 함수칸에 넣으면, 2의 배수인 녀석들만 남는데,**  
**None을 넣을시 빈 값(None)만이 모두 사라진다.**    
**뭔가 이상했다.**  


---
## 특이점에 대한 해답

![ㅅㄱㄷㅅ](https://user-images.githubusercontent.com/34790699/95190887-ec2d0280-080a-11eb-8a7c-52c60f661168.png)
> + **If function is None, the identity function is assumed, that is, all elements of iterable that are false are removed.**  
> + **함수가 None이라면 , 기본 함수는 가정되는데 그것은 false가 제거된 제거된 모든 반복된 원소들이다.**

---
## 회고
+ 되게 희안했다. 함수로 None이 오면 '아무것도 없는 것들을 필터링'해야 할 것 같지만, 거꾸로 '있는 것들'이 필터링되어 나온다니..
+ 가장 먼저, 파이썬 공식문서를 찾아보는 습관을 가지자.
+ [Stack Overflow Link](https://stackoverflow.com/questions/38339496/python-filternone-list-of-bools-behavior)