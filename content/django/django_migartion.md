---
title: "Django - Migration을 통한 database 스키마 관리"
date: 2020-05-05T14:14:58+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Migration', 'Migrate']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### Migrations
+ 모델의 변경내역을 "데이터베이스 스키마"로 반영시키는 효율적인 방법 제공

### 관련 명령:
###### 마이그레이션 파일 생성
~~~
> python manage.py makemigrations <앱이름>
~~~
###### 지정 데이터베이스에 마이그레이션 적용
~~~
> python manage.py migrate  <앱이름>
~~~
###### 마이그레이션 적용 현황 출력
~~~
> python manage.py showmigrations <앱이름>
~~~
###### 지정 마이그레이션의 SQL 내역 출력
~~~
> python manage.py sqlmigrate <앱이름> <Migration 이름>
~~~
---
### Migration File
> 의도에 맞게 Migration 파일이 생성되었는지 확인하는 것이 무엇보다 중요

###### 기능
+ Migration File은 모델의 변경내역을 누적하는 역할
+ 데이터베이스에 어떤 변화를 가하는 Operation들을 나열
+ 테이블 생성/삭제, 필드 추가/삭제 등
+ Custom Python/SQL Operation (data migration 등)
+ makemigrations 명령에 의해 모데로부터 자동 생성
+ 모델 참조 없이 빈 Migration File을 만들어서 직접 채워 넣는 것도 가능

###### 주의할 점
+ 같은 Migration 파일이라도 DB 종류에 따라 다른 SQL이 생성됨
+ 모든 데이터 베이스 엔진들이 같은 기능을 제공하지는 않음
+ **적용된 마이그레이션 파일은 절대로 삭제해서는 안됨** 

---
### makemigrations의 타이밍

+ 모델 필드 관련된 어떠한 변경이라도 발생 시에 마이그레이션 파일 생성
+ 실제로 Db Scheme 에 가해지는 변화가 없더라도 수행



---
### Migrations의 Migrate (정/역 방향)
~~~
> python manage.py migrate <앱이름>
~~~
+ 미적용 <Migration-File>부터 최근 <Migration-File>까지 정방향으로 순차적으로 수행
~~~
> python manage.py migrate <앱이름> <Migration-이름>
~~~
+ 지정된 <Migration-이름>이 현재 적용된 Migration보다
  1. 이후라면, 정방향으로 순차적으로 지정 마이그레이션까지 forward 수행
  2. 이전이라면, 역방향으로 순차적으로 지정 마이그레이션 이전까지 backward 수행
---
### Migration 이름 지정
 전체 이름(파일명)을 지정 하지 않더라도, 1개를 판별할 수 있는 일부만 지정해도 OK
###### 코드 예시
~~~
 - account/migrations/0001_initial.py
 - account/migrations/0002_create_field.py
 - account/migrations/0002_update_field.py
~~~
###### Success 사례
~~~
1. > python manage.py migrate blog 0001
2. > python manage.py migrate blog 0002_create 
3. > python manage.py migrate blog 0002_c 
~~~

###### Fail 사례
~~~
1. > python manage.py migrate blog 0002
2. > python manage.py migrate blog 000 
3. > python manage.py migrate blog 100
~~~
1. 2개의 파일에 매칭되므로 Fail
2. 3개의 파일에 매칭되므로 Fail
3. 매칭되는 파일이 없으므로 Fail

###### 기타
~~~
 > python manage.py migrate blog zero
~~~
  
account 앱의 모든 상황을 롤백 (최초의 상황으로 회귀)
  
---
  
### Migration 순서
  
> 보기 좋기 위해 파일명 순으로 되어 있으나 정확하게는 
Migration 파일의 dependencies에 따라서 정의되는 형태

~~~
from django.db iport migrations, models
class Migration(migration.Migration):
  dependencies = [
 ('shop, '0001_initial'),
]
operations = [
...
]
~~~
---
### id 필드가 생기는 이유
+ 모든 DB 테이블에는 각 Row의 식별기준인 "기본키 (Promary Key)"가 필요   
+ 장고에서는 기본키로서 id ( AutoField) 필드로 디폴트를 생성. Record의 주소 역할
+ 다른 필드를 기본키로 지정하고 싶다면 해당 필드에 primary_key=True 옵션 적용
---
### 모델 클래스의 필수필드 설정

###### 필수필드 여부
+ 필드옵션 blank, null의 default값이 False. 기본적으로 모든 필드는 필수 필드
+ makemigrations 명령을 수행할 때, 기존 Record들에 대해서 어떤 값을 채워 넣을지 설정 필요
+ 특정 모델에 대한 첫 Migration의 경우는 문제가 없으나 추후 새로운 필드를 추가하고자 할 때, 그 필드가 필수필드라면 아래와 같은 질문이 보여짐

> you are tying to add a non-nullable field 'example' to post withaout a default; we can't do that (the database needs something to populate existing rows).
please select a fix:

  + 선택1 : 지금 값을 입력   
  + 선택2 : 명령 수행을 중단, 모델 클래스를 수정하여 default값 제공
---
### 협업시 TIP
+ 팀원 각자가 마이그레이션 파일 생성시 충돌 발생 ( 절대 금지 )  
-> **Migration File 생성은 1명이 전담해서 생성 ( 추천 )**
+ 생성한 Migrarion File을 버전관리에 넣고, 다른 팀원들은 이를 받아서 migrate만 수행
---
### 개발시 TIP
##### 서버에 아직 반영하지 않은 Migration을 다수 생성했을 경우
+ 이것들을 그대로 서버에 반영(migrate)하지 않고, 하나의 Migration으로 합쳐서 적용하기를 권장  
+ 추천 방법 : 서버로의 미적용 Migration들을 모두 롤백하고, 롤백된 Migration을 모두 제거, 새로이 Migration File 생성
+ 장고 모델의 migration은 스키마의 형상관리만 하기 때문에 데이터 백업을 지원하지 않으므로 주기적인 백업이 필요
+ 또다른 방법 : 미적용 Migration들을 하나로 합치기, squashmigrations 명령 사용

