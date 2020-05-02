---
title: "Django Model을 통한 조회 (기초)"
date: 2020-04-30T11:59:38+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Model']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### Model Manager
SELECT * FROM app_model;
~~~
ModelCls.objects.all()
~~~
SELECT * FROM app_model ORDER BY id DESC LIMIT 10;
~~~
ModelCls.objects.all().order_by('-id')[:10]
~~~
INSERT INTO app_model (title) VALUES ("New Title");
~~~
ModelCls.objects.create(title="New Title")
~~~
---
### Queryset

###### Chaining을 지원
> Post.objects.all().filter(...).exclude(...).filter(...) -> QuerySet

###### Lazy한 특성
>  QuerySet을 만드는 동안에 DB 접근 X  
> 실제로 데이터가 필요한 시점에 접근
###### 데이터가 필요한 시점
  1. queryset
  2. print(queryset)
  3. list(queryset)
  4. for instance in queryset: print(instance)
> **"I/O의 경우 파일 접근은 많이 하지 않고, 대개 데이터베이스 접근이기 때문에, DB QuerySet에 대한 명확한 이해가 성능향상을 도움"**
---
### 다양한 조회요청 방법 - SELECT SQL 생성
###### 조건을 추가한 Queryset, 획득할 준비

+ queryset.filter(...)  
+ queryset.exclude(...)
###### 특정 모델객체 1개 획득을 시도

+ queryset[Number Index]->모델객체 혹은 예외발생(IndexError)  
**Index 범위를 벗어날 시 예외발생**  

+ queryset.get(...)->모델객체 혹은 예외발생(DoesNotExist, MultipleObjectsReturned)
**존재하지 않거나, 값이 여러개일때 에러 발생**
+ queryset.first() -> 모델객체 혹은 None 
+ queryset.last() -> 모델객체 혹은 None
---
### filter와 exclude (반대) - SELECT 쿼리에 WHERE 조건 추가
+ 인자로 "필드명 = 조건값" 지정
+ 1개 이상의 인자 지정 -> 모두 **AND 조건**으로 묶임
+ **OR 조건을 묶기 위해서는, django.db.models.Q 활용**

###### 기본 - and 조건
~~~
Item.objects.filter(name-"New Item", price=3000)
~~~
###### q 사용법 - or 조건 
~~~
from django.db.models import Q
qs = qs.filter((Q(id_gte=2) | Q(message__icontains=query)) # 비트 연산자의 or(|)를 사용
~~~
###### q 사용예시
~~~
cond = Q(id_gte=2) | Q(message__icontains=query
qs = qs.filter(cond)
~~~
---
### 필드 타입별 다양한 조건 매칭
> **주의!** 데이터베이스에 따라 생성되는 SQL이 다름
###### 숫자/날짜/시간 필드
|필드 타입|조건 매칭|
|-|-|
필드명__lt = 조건값 | 필드명 < 조건값 
필드명__lte = 조건값 | 필드명 <= 조건값 
필드명__gt = 조건값 | 필드명 > 조건값 
필드명__gte = 조건값 | 필드명 >= 조건값   
    
    
###### 문자열 필드
|필드 타입|조건 매칭|
|-|-|
필드명__startswith = 조건값 | 필드명 LIKE "조건값%"
필드명__istartswith = 조건값 | 필드명 ILIKE "조건값%"
필드명__endswith = 조건값 | 필드명 LIKE "%조건값"
필드명__iendswith = 조건값 | 필드명 ILIKE "%조건값"
필드명__contains = 조건값 | 필드명 LIKE "%조건값%"
필드명__icontains = 조건값 | 필드명 ILIKE "%조건값%"
---
### 메모
###### 아래와 같은 형식으로도 사용이 가능하니 알아두자
~~~
for post in qs:
print("id : {id}, message : {message} ({created_at})".format(**post.__dict__)) 
~~~

