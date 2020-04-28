---
title: "Django의 Admin 기능"
date: 2020-04-19T18:03:29+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Admin', 'Python']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### 1. Django의 Admin 기능
+ 관리자 페이지, 강력한 자동 관리 인터페이스
+ 모델 중심의 빠른 인터페이스를 제공   
---
  
### 2. Django Admin의 사용 효과
+ 서비스 초기에 관리 도구로 사용하기에 제격
+ 관리 도구 만들 시간을 줄이고, End-User 서비스에 집중이 가능
---
### 3. Model Class를 Django Admin에 등록하는 방법  
  
###### 1. 기본
~~~
from django.contrib import admin
from .models import Item 
  
admin.site.regiester(Item)
~~~
  
   
   
###### 2. Class
~~~
from django.contrib import admin
from .models import Item
  
class ItemAdmin(admin.ModelAdmin):
	pass
admin.site.register(Item, ItemAdmin) 
~~~
	
	
	
###### 3. Class + Decorator	(선호)
~~~
from django.contrib import admin
from .models import Item

@admin.register(Item)
class ItemAdmin(admin.ModelAdmin):
	pass
~~~
---
### 4. 제공되는 기능
|이름|기능|비고|
|---|----|---|
|list_display|컬럼의 노출여부를 결정|function도 가능|
|list_display_links|컬럼의 링크 설정 (기본값은 1번째 컬럼)|여러개 허용|
|search_fields|admin 내 검색 UI 제공|
|list_filter|지정 필드값으로 필터링 옵션 UI 제공|
...
###### 구현 형태
~~~
admin.py

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
   
   list_display = ['title', 'content', 'title_length', 'is_public', 'created_at', 'updated_at'] 
   list_display_links = ['content'] 
   search_fields = ['title']   
   list_filter = ['created_at', 'is_public']
   
   def title_length(self, post):
      return f"{len(post.title)}글자"
	  
   title_length.short_description = "타이틀 글자수"
~~~
###### 추가 설명
---
> **title_length**
+ function도 가능 (list_display 비고란 참고)
+ list_display 내의 원소이름과 동일해야함.
+ model.py에서 같은 구현 가능
+ python의 경우 @property로도 정의
  
---  
> **title_length.short_description**
+ 컬럼의 표시되는 부분을 설정
  
---
> **search_fileds**

~~~
**로직상의 의미**
Post.objects.all().filter(title__icontains='포함하는 글자')
~~~
+ search_field = ['title']의 경우, 'title'에 대해서 search 옵션을 적용하겠다는 의미.

---
+ ###### 더 많은 기능에 대한 정보는 공식문서를 참고. (https://docs.djangoproject.com/en/3.0/ref/contrib/admin/)
---


