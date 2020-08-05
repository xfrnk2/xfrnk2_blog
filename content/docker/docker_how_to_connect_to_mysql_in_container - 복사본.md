---
title: "[Docker] 컨테이너 내 Mysql서버에 접근하는 방법"
date: 2020-08-05T10:36:16+09:00
draft: false

categories: ['docker']
tags: ['docker']
author: "xfrnk2"
---
### 1. Docker Dashboard의 CLI 이용
![K-016](https://user-images.githubusercontent.com/34790699/89434469-8a0a3f80-d77e-11ea-889f-9534f5801ae5.png)    
  
▶ Docker Dashboard의 container를 보면 왼쪽에서 두번째 위치에 CLI 버튼이 있다. 위에 마우스를 가져다 대고 있으면 CLI라는 표시가 보인다.  
  
▶ CLI 아이콘 클릭시 우측과 같은 창이 뜨게 되는데, 아래와 같이 커맨드를 입력한다.
  
~~~
mysql -u root -p
~~~
  
▶ 그 다음 지정한 password를 입력하면 mysql에 접속할 수 있다.


---
  
### 2. CMD 또는 터미널 이용
▶ 커맨드 입력 
~~~
docker exec -it (container id) bash
~~~
  
▶ 커맨드 입력 
~~~
mysql -u root -p
~~~
그 다음 지정한 password를 입력하면 mysql에 접속할 수 있다.