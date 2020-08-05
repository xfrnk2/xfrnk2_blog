---
title: "[MYSQL] Error: Authentication plugin ‘caching_sha2_password’ cannot be loaded 해결"
date: 2020-08-05T10:36:16+09:00
draft: false

categories: ['mysql']
tags: ['database', 'mysql']
author: "xfrnk2"
---

▶ MySQL 8.0부터는 default_authentication_plugin 이 mysql_native_password 에서 caching_sha2_password 로 변경되었는데, 강화된 보안 체계로 인해 외부 어플리케이션에서 사용되는 mysql 관련 모듈이 mysql 8.x 의 기본 값으로 설정된 패스워드 보안 알고리즘을 맞추지 못하여 발생하는 에러이니 mysql 인스턴스 전체에 영향을 주지 않는 방법으로 사용자 단위로 패스워드 보안 정책을 변경하는 편이 가장 좋다고 한다.  

▶ stackoverflow를 검색해보면 바로 나오는 것들이지만 빠르게 찾아보기 용이하게 하기 위해 간단히 방법만을 기록해 둡니다. 아래 방법중 어느것인가를 사용하면 에러를 해결했던 것 같습니다.
  
---
  
##### 방법 1 : Query 실행  
~~~
ALTER USER 'yourusername'@'localhost' IDENTIFIED WITH mysql_native_password BY 'youpassword';
~~~
  
---
  
##### 방법 2 : my.ini 내용 변경  
~~~
C:\ProgramData\MySQL\MySQL Server 8.0\my.ini
~~~
해당 경로의 파일을 열어서, default_authentication_plugin 항목을 찾은 다음,
~~~
default_authentication_plugin=caching_sha2_password
~~~  
caching_sha2_password 에서  
~~~
default_authentication_plugin=mysql_native_password
~~~
mysql_native_password 으로 변경 후 저장  
  
---
  
##### 방법 3 : 커맨드 옵션 추가(example: docker)
~~~
"default-authentication-plugin=mysql_native_password" 
~~~
    
---
  

참고 사이트:  
https://mysqlserverteam.com/upgrading-to-mysql-8-0-default-authentication-plugin-considerations/
https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html
http://minsql.com/mysql8/C-2-A-authentificationPlugin/
https://ondemand.tistory.com/246