---
title: "Django - Model Field"
date: 2020-05-02T23:19:28+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Model']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---

### ORM
+ ORM은 어디까지나, SQL 생성을 도와주는 라이브러리
+ DB에 대한 모든 것을 알아서 처리해주진 않음
> 보다 성능높은 어플리케이션을 만들고자 한다면 사용할 DB엔진과 SQL에 대한 높은 이해 필요
  
  
---
### RDMBS에서의 관계 예시
###### 1 : N 관계 -> Foreign Key
+ 1명의 유저(User)가 쓰는 다수의 포스팅(Post)
+ 1명의 유저(User)가 쓰는 다수의 댓글(Comment)
+ 1개의 포스팅(Post)에 다수의 댓글(Comment)
   
###### 1 : 1 관계 -> models.OneToOneField
+ 1명의 유저(User), 1개의 프로필(Profile)
   
   
###### M : N 관계 -> models.ManyToManyField
+ 1개의 포스팅(Post), 다수의 태그(Tag)
+ 1개의 태그(Tag), 다수의 포스팅(Post)