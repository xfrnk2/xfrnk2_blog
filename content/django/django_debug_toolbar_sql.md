---
title: "Django debug-toolbar를 통한 SQL 디버깅"
date: 2020-05-02T22:12:14+09:00
draft: false
featuredImage: "https://example.org/images/my-image.jpg"
categories: ['Django']
tags: ['Django', 'debug-toolbar', 'SQL']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### django-debug-toolbar

+ 현재 request/response에 대한 다양한 디버깅 정보를 보여주고 다양한 패널을 지원
+ 응답 포맷이 HTML일 때만 가능
+ SQLPanel을 통해 각 요청 처리시에 발생한 SQL 내역을 확인 가능
+ Ajax 요청에 대한 지원은 불가능
---
### django-debug-toolbar 설치
###### 설치 공식문서
+ https://django-debug-toolbar.readthedocs.io/en/latest/installation.html 
###### 주의사항
+ 웹페이지의 템플릿에 필히 body 태그가 있어야만, django-debug-toolbar가 동작
> 이유 : dbt의 html/script 디폴트 주입 타겟이 /body 태그 (INSERT_BEFORE 설정 디폴트: "/body")
+ https://django-debug-toolbar.readthedocs.io/en/latest/configuration.html#toolbar-options
---
### 코드를 통한 SQL 내역 확인
###### QuerySet의 query 속성 참조
+   ex) print(Post.objects.all().query) → 실제 문자열 참조 시에 SQL 생성
###### settings.DEBUG = True 시에만 쿼리 실행내역을 메모리에 누적
~~~
from django.db import connection, connections

for row_dict in connection.queries:
    print('{time} {sql}'.format(**row_dict))
connections['default'].queries
~~~
  
###### 쿼리 초기화
+ 메모리에 누적되기에, 프로세스가 재시작되면 초기화
+ django.db.reset_queries() 통해서 수동 초기화도 가능

---
### 그 외 - django-querycount 라이브러리
+ SQL 실행내역을 개발서버 콘솔 표준출력, Ajax 내역도 출력 가능
+ https://github.com/bradmontgomery/django-querycount/
