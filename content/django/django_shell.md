---
title: "Django Shell"
date: 2020-04-29T14:27:36+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Shell']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---  
### Shell
  
+ Ipython : 기본 패키지에 포함되지 않음, 별도의 설치가 필요
+ Jupyter Notebook : Ipython의 웹버전, 웹상에서 코드가 동작, 이미지와 차트 등이 활용 가능, 분석 또는 머신러닝에서 사용
+ BPython
  
---

### 장고 프로젝트 설정이 로딩된 파이썬 쉘

+ 쉘 : python manage.py shell

+ 우선순위 : ipython 쉘, bpython 쉘, python 쉘

###### 옵션 :

+ -i (--interface) : 인터프리터 인터페이스 커스텀 지정

+ -c (--command) : 실행할 파이썬 코드를 문자열로 지정
  
---
###### 예시
~~~  
python -c "print("2**100")" # 쉘로 진입하지 않음
python manage.py shell -c "print("2**100") # manage.py 쉘에서도 동일하게 지원
~~~
###### 유사한 예
~~~
echo print(2**100) | python
echo print(2**100) | python manage.py shell -c
~~~
---
### 장고가 시작될 때 꼭 필요한 환경 변수

~~~
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'django_tech.settings')
~~~

+ 장고가 시작될 때 꼭 필요한 환경변수, settings의 위치를 지정
+ wsgi나 asgi, manage.py 등 파일 내부에서 확인 가능  
###### 현재 프로세스에서 환경변수 목록
~~~
import os 
os.environ
~~~
  
접근 방법 예시
  
~~~
import os
os.environ['DJANGO_SETTINGS_MODULE'] = 'django_tech.settings'
os.environ["DJANGO_ALLOW_ASYNC_UNSAFE"] = "true" # 주피터 노트북의 경우
import django
django.setup()
from blog1.models import Post
qs = Post.objects.all()
print(qs.query)
~~~

###### 결과
~~~
SELECT "blog1_post"."id", "blog1_post"."title", "blog1_post"."content", "blog1_post"."created_at", "blog1_post"."updated_at", "blog1_post"."photo", "blog1_post"."is_public" FROM "blog1_post"
~~~
+ 주피터 노트북에서도 과정은 위와 동일하나 별도의 옵션 필요
~~~
os.environ["DJANGO_ALLOW_ASYNC_UNSAFE"] = "true"
~~~
+공식 문서
https://docs.djangoproject.com/en/3.0/topics/async/

---

### 주피터노트북의 활용방법
+ 분석이나 크롤링 같은 작업을 통해서 모델을 저장하는 경우 등 장고 모델과 연계해서 분석 같은 것을 하고자 할때,
주피터 노트북은 테스트를 위한 편리한 환경으로서 사용이 가능

