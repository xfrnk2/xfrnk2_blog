---
title: "Django - 관계를 표현하는 Model Field - OneToOneField"
date: 2020-05-04T11:11:59+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Model']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### OneToOneField
  
###### 1 : 1 관계에서 어느 쪽이라도 가능
+ User:Profile
###### ForeignKey(unique_True)와 유사하지만, reverse 차이
+ User:Profile를 ForeignKey로 지정한다면 -> profile.user_set.first() -> user
+ User:Profile를 OneToOneField로 지정한다면 -> profile.user -> user
  
  
###### OneToOneField(to, on_delete)
~~~
#django/contrib/auth/models.py
class User(abstractBaseUser):
  ...
  
# accounts/models.py
class Profile(models.Model):
	author = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
~~~

https://docs.djangoproject.com/en/3.0/ref/models/fields/#onetoonefield

---
### OneToOneField에서의 related_name
###### reverse 접근 시의 속성명 : 디폴트 -> 모델명소문자

~~~
accounts/models.py
class Profile(models.Model):
	user = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
	phone = models.CharField(max_length=11, blank=True)
	birth = models.DataField(null=True)
	
>>> profile.user

>>> user.profile	
~~~
---
### 유저 모델을 얻어오는 방법
###### 권장하지 않는 방법
~~~
from django.contrib.auth.models import User
User.objects.all()
~~~
###### 추천하는 방법
~~~
from django.contrib.auth import get_user_model
User = get_user_model()
user = User.objects.first()
user.profile
~~~
+ + 현재 활성화된 유저 모델을 얻을 수 있는 함수 : **get_user_model**
---
### 기타
~~~
profile.delete() #삭제
~~~