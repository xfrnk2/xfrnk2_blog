---
title: "Django - Queryset의 정렬 및 범위조건"
date: 2020-05-06T12:55:27+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Model', 'Queryset']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### 정렬 조건 추가
+ 일관된 순서를 보장받기 위해 정렬 조건을 추가
+ DB에서 다수 필드에 대한 정렬을 지원하나 가급적 단일 필드로 하는것이 성능에 이익
+ 시간순/역순 정렬이 필요할때는 id필드를 활용
---
### 정렬 조건 지정 방법
###### 1. (추천) 모델 클래스의 Meta 속성으로 ordering 설정 : list로 지정  
###### 2. 모든 queryset에 order_by(...)에 지정
> (주의) queryset코드에서 직접 order_by를 지정하면, Meta>ordering 설정값이 무시된다
---
###### django-extensions
+ 커스텀 확장 기능 제공 라이브러리
+ Management Command, additional database fields, admin extensions 등을 지원
~~~
> python manage.py shell_plus --print-sql
~~~

---
### 정렬 지정하기 #1
~~~
class Item(Models.Model):
	name = models.CharField(max_length=100)
	desc = models.TextField(blank=True)
	price = models.PositiveIntegerField()
	created_at = models.DateTimeField(auto_now_add=True)
	updated_at = models.DataTimeField(auto_now=True)
	
	class Meta:
		ordering = ['id']
~~~

### 정렬 지정하기 #2
~~~
#1
In [4]: Post.objects.all().order_by('id')
Out[4]: SELECT "blog1_post"."id",
       "blog1_post"."author_id",
       "blog1_post"."title",
       "blog1_post"."content",
       "blog1_post"."created_at",
       "blog1_post"."updated_at",
       "blog1_post"."photo",
       "blog1_post"."is_public"
  FROM "blog1_post"
 ORDER BY "blog1_post"."id" ASC
 LIMIT 21

Execution time: 0.000000s [Database: default]
<QuerySet [<Post: 첫번째 제목>, <Post: 두번째 제목>, <Post: 세번째 제목>]>


#2
In [5]: Post.objects.all().order_by('-id')
Out[5]: SELECT "blog1_post"."id",
       "blog1_post"."author_id",
       "blog1_post"."title",
       "blog1_post"."content",
       "blog1_post"."created_at",
       "blog1_post"."updated_at",
       "blog1_post"."photo",
       "blog1_post"."is_public"
  FROM "blog1_post"
 ORDER BY "blog1_post"."id" DESC
 LIMIT 21

Execution time: 0.000000s [Database: default]
<QuerySet [<Post: 세번째 제목>, <Post: 두번째 제목>, <Post: 첫번째 제목>]>

#1과 2의 차이
1 정순-> ORDER BY "blog1_post"."id" ASC
2 역순->  ORDER BY "blog1_post"."id" DESC
~~~

---
### 범위 조건
+ 슬라이싱을 통해서 SELECT 쿼리에 "OFFSET/LIMIT" 추가 기능 
+ str/list/tuple에서의 슬라이싱과 거의 유사하나, **음수 인덱스 접근 슬라이싱은 지원하지 않음
(데이터베이스에서 지원하지 않기 때문)**

###### 형태
> **객체[start:stop:step]**
> + Offset -> Start
> + Limit -> start  
> + **Step이 들어가는 순간 반환값의 타입이 queryset-> list가 된다**   
> -> (쿼리에 대응되지 않으므로 사용을 비추천)

### 코드 예시
  
   
###### 음수 인덱스 접근시 에러 발생
~~~
In [7]: Post.objects.all().order_by("-id")[:-1]
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-7-b98dbbba47d0> in <module>
----> 1 Post.objects.all().order_by("-id")[:-1]

C:\Program Files\python\lib\site-packages\django\db\models\query.py in __getitem__(self, k)
    288                 % type(k).__name__
    289             )
--> 290         assert ((not isinstance(k, slice) and (k >= 0)) or
    291                 (isinstance(k, slice) and (k.start is None or k.start >= 0) and
    292                  (k.stop is None or k.stop >= 0))), \

AssertionError: Negative indexing is not supported.
~~~

###### 객체[start:stop] : Queryset 타입 반환
~~~
In [9]: Post.objects.all().order_by("-id")[1:2]
Out[9]: SELECT "blog1_post"."id",
       "blog1_post"."author_id",
       "blog1_post"."title",
       "blog1_post"."content",
       "blog1_post"."created_at",
       "blog1_post"."updated_at",
       "blog1_post"."photo",
       "blog1_post"."is_public"
  FROM "blog1_post"
 ORDER BY "blog1_post"."id" DESC
 LIMIT 1
OFFSET 1

Execution time: 0.000000s [Database: default]
<QuerySet [<Post: 두번째 제목>]>		      
~~~
###### 객체[start:stop:step] : list 타입 반환
~~~
In [10]: Post.objects.all().order_by("-id")[1:2:1]
SELECT "blog1_post"."id",
       "blog1_post"."author_id",
       "blog1_post"."title",
       "blog1_post"."content",
       "blog1_post"."created_at",
       "blog1_post"."updated_at",
       "blog1_post"."photo",
       "blog1_post"."is_public"
  FROM "blog1_post"
 ORDER BY "blog1_post"."id" DESC
 LIMIT 1
OFFSET 1

Execution time: 0.000000s [Database: default]
Out[10]: [<Post: 두번째 제목>]
~~~
