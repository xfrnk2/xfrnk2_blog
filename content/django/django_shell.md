---
title: "장고 쉘"
date: 2020-04-29T14:27:36+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Shell']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---  
### Shell
  
+ Ipython은 기본 파이썬 패키지에는 없고 pip install 또는 콘다 install을 통해서 설치 가능하다.
+ Jupyter Notebook은 Ipython의 웹버전이다. 웹브라우저를 통해 사용한다.
+ 웹상에서 코드가 동작하므로 글자 이외의 이미지나 차트 등의 여러 intertive한 UI들을 아웃풋으로 내어 줄 수 있다.
+ 그러므로 분석이나 머신러닝 등에서 사용하는 편이다.
+ BPython도 있다.
  
---

### 장고 프로젝트 설정이 로딩된 파이썬 쉘

+ 쉘> python manage.py shell

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
+ wsgi나 asgi, manage.py 파일에 가보면 아래와 같이 나와있다.
~~~
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'django_tech.settings')
~~~
+ 장고가 시작될 때 꼭 필요한 환경변수이다.
+ 장고 setting의 위치를 알려달라는 것이다. 알려 줘야지만 장고에 필요한 설정을 읽어들일 것이기 때문이다.
  
###### 현재 프로세스에서 환경변수 목록
~~~
import os 
os.environ
~~~
  
아래와 같은 형태로 접근이 가능하다.
  
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
+ 주피터 노트북에서도 과정은 위와 동일하다.

+ 주피터 노트북에서 사용하고자 한다면 어떻게 해야하는가에 대한 내용이 나와있다.  
https://docs.djangoproject.com/en/3.0/topics/async/

---

### 주피터노트북의 활용방법
+ 분석이나 크롤링 같은 작업을 통해서 모델을 저장하는 경우 등 장고 모델과 연계해서 분석 같은 것을 하고자 할때,
주피터 노트북은 테스트를 하기 위한 편리한 환경이 된다.
  
+ 그때 그때 파이참이나 Vscode와 같은 프로그램을 사용해서 실행을 하는 방법도 있겠지만, 주피터 환경을 사용하면 좀더 편리한 사용이 가능하다.

