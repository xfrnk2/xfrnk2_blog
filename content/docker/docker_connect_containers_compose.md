---
title: "[Docker]docker-compose을 사용한 Container 단위 Django 서버와 Mysql 서버 연동"
date: 2020-08-05T10:36:16+09:00
draft: false

categories: ['docker']
tags: ['docker']
author: "xfrnk2"
---

▶ [docker-compose에 대한 공식 문서 링크](https://docs.docker.com/compose/)
      
---
  	
#### 1. 파일 생성 및 작성 
![K-002](https://user-images.githubusercontent.com/34790699/89426684-62fb4000-d775-11ea-97f5-05bdff6928cb.png)  
dockerfile,  
docker-compose.xml,  
requirements.txt    
  
각각의 파일들의 작성방법에 대한 내용은 이 글에서는 다루지 않으므로 생략합니다.  
기본적인 기능으로서 각각의 파일들을 설정한 상태는 아래와 같습니다.
##### ▼ dockerfile
~~~
FROM python:3.8.1

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN mkdir /code
ADD . /code/

RUN usr/local/bin/python -m pip install --upgrade pip

WORKDIR /code/
RUN pip install -r requirements.txt


~~~
  

    

##### ▼ docker-compose.xml
  
~~~
version: '3.8'

services:
    db:
        image: mysql
        command: mysqld --default-authentication-plugin=mysql_native_password
        restart: always
        environment:
            MYSQL_DATABASE: rad_db
            MYSQL_ROOT_PASSWORD: password 
        ports:
          - "3306:3306"
            
    web:
        build: .
        command: python manage.py runserver 0.0.0.0:8000
        volumes:
          - .:/code
        ports:
          - "8000:8000"
        depends_on:
          - db
          
~~~
  
> command : "default-authentication-plugin=mysql_native_password"  
　  
▶ 이것을 선언하지 않으면 Authentication plugin 'caching_sha2_password' cannot be loaded라는 에러가 발생한다.      
　  
▶ MySQL 8.0부터는 default_authentication_plugin 이 mysql_native_password 에서 caching_sha2_password 로 변경되었는데, 강화된 보안 체계로 인해 외부 어플리케이션에서 사용되는 mysql 관련 모듈이 mysql 8.x 의 기본 값으로 설정된 패스워드 보안 알고리즘을 맞추지 못하여 발생하는 에러이니 mysql 인스턴스 전체에 영향을 주지 않는 방법으로 사용자 단위로 패스워드 보안 정책을 변경하는 편이 가장 좋다고 한다.  
　  
관련 참고 사이트    
https://mysqlserverteam.com/upgrading-to-mysql-8-0-default-authentication-plugin-considerations/  
https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html  
http://minsql.com/mysql8/C-2-A-authentificationPlugin/  
https://ondemand.tistory.com/246  
  

  
##### ▼ requirements.txt
  
~~~
Django==3.0.3
mysqlclient==1.4.6
~~~

---
   
#### 2. Django 프로젝트를 생성
~~~
docker-compose run web django-admin.py startproject (project_name) .
~~~
  
![K-003](https://user-images.githubusercontent.com/34790699/89429323-62b07400-d778-11ea-98cc-339b0cead59c.png)
![K-004](https://user-images.githubusercontent.com/34790699/89429331-647a3780-d778-11ea-96ef-b4453a9f1632.png)
▶ 모든 커맨드 및 의존성 설치를 순차적으로 수행  
  
---
  
#### 3. settings.py 수정
~~~
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'rad_db',
        'USER': 'root',
        'PASSWORD': 'password',
        'HOST': 'db',
        'PORT': '3306',
    }
}
~~~
---
#### ▶ 연동 완료, 127.0.0.1:8000 접속 가능
![K-012](https://user-images.githubusercontent.com/34790699/89432878-97bec580-d77c-11ea-8ff2-19d58e12452f.png)
▶ mysql 접속시 위에서 선언한 rad_db가 생성된 것을 확인 가능


 
> ["[Docker]컨테이너 내 Mysql서버에 접근하는 방법" 바로가기 -- link](http://xfrnk2.github.io/docker/docker_connect_containers_compose/)
