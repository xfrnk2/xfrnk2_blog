---
title: "[Docker] network를 사용한 Container 단위 Django 서버와 Mysql 서버 연동"
date: 2020-08-05T10:36:16+09:00
draft: false

categories: ['docker']
tags: ['docker']
author: "xfrnk2"
---
### 1. 네트워크 생성
~~~
docker create network (network name)
~~~
  
▶ 원하는 네트워크 이름을 넣을 수 있다.
  
~~~
docker network ls
~~~
▶ 네트워크가 생성되었음을 확인할 수 있다.
  
---
  
### 2. 네트워크 연결
▶ network와 container 모두 해당 id나 name을 입력할 수 있음
~~~
docker network connect (network) (container)
~~~
  
▶ network 조회:
~~~
docker network ls  
~~~
  
▶ container 조회:
~~~
docker ps -a
~~~
  
---
  

### 3. mysql container 생성

~~~
docker run -e MYSQL_ROOT_PASSWORD=password -d -p 3306:3306 --name=(name) mysql --default-authentication-plugin=mysql_native_password
~~~
  
▶ (name)은 mysql container에 부여할 이름이다. 알맞게 정해주도록 한다.
  
  
> **command : "default-authentication-plugin=mysql_native_password"**  
　  
▶ 이것을 선언하지 않으면 Authentication plugin 'caching_sha2_password' cannot be loaded라는 에러가 발생한다.      
　  
▶ MySQL 8.0부터는 default_authentication_plugin 이 mysql_native_password 에서 caching_sha2_password 로 변경되었는데, 강화된 보안 체계로 인해 외부 어플리케이션에서 사용되는 mysql 관련 모듈이 mysql 8.x 의 기본 값으로 설정된 패스워드 보안 알고리즘을 맞추지 못하여 발생하는 에러이니 mysql 인스턴스 전체에 영향을 주지 않는 방법으로 사용자 단위로 패스워드 보안 정책을 변경하는 편이 가장 좋다고 한다.  
　  
관련 참고 사이트    
https://mysqlserverteam.com/upgrading-to-mysql-8-0-default-authentication-plugin-considerations/  
https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html  
http://minsql.com/mysql8/C-2-A-authentificationPlugin/  
https://ondemand.tistory.com/246  
  
  
### 4. 실행중인 mysql container에 접속하여 db 생성  
> ["[Docker]컨테이너 내 Mysql서버에 접근하는 방법" 바로가기 -- link](http://xfrnk2.github.io/docker/docker_connect_containers_compose/)

~~~
CREATE database (db_name);
~~~
▶ (db_name) : 원하는 이름으로 database를 만든다.
~~~
SHOW databases;
~~~
▶ 위의 커맨드로 생성여부를 확인 가능
   
---
  
### 5. Django container 생성
▶ dockerfile을 사용하여 빌드한 이미지를 컨테이너로 실행   
▶ settings.py의 DATABASES를 mysql container와의 상호작용에 알맞게 수정 ↓  
~~~
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '(db_name)',
        'USER': 'root',
        'PASSWORD': 'password',
        'HOST': '(name)',
        'PORT': '3306',
    }
}
~~~
▶ **3번 과정에서 정한 (name)을 'HOST'로 설정**  
▶ **4번 과정에서 정한 (db_name)을 'NAME'으로 설정**

▶ Django container와 Mysql container를 재실행하면 정상 동작하는것을 확인 가능