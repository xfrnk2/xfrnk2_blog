---
title: "Django - 관계를 표현하는 Model Field - ManyToManyField"
date: 2020-05-04T12:24:08+09:00
draft: false

categories: ['Django']
tags: ['Model']
author: "xfrnk2"
---
### ManyToManyField
###### M : N 관계에서 어느쪽이라도 필드 지정 가능
###### ManyToManyField(to, blank=Flase) 
+ on_delete 옵션이 없고 blank라는 옵션을 **유의미하게** 사용  
  예 ) 태그 지정에 대한 유효성 검사
+ ForeignKey 관계와 OneToOneField 관계에서는 두개의 모델만 있으면 충분하지만,   
  ManyToManyField 관계에서는 두개의 테이블만으론 관계를 정의할 수 없기에 중간 테이블이 필요 (별도의 지정 및 정의도 가능)  
    

###### 방법 1 (권장)
~~~
class Post(models.Model):
	tag_set = models.ManyToManyField('Tag', blank=True)

class Article(models.Model):
	tag_set = models.ManyToManyField('Tag', blank=True)

class Tag(models.Model):
	name = models.CharField(max=length=100, unique=True)
~~~
###### 방법 2
~~~
class Post(models.Model):
    ...
class Article(models.Model):
    ...
class Tag(models.Model):
	name = models.CharField(max_length=100, unique = True)
	post_set = models.ManyToManyField('Post', blank = True)
	article_set = models.ManyToManyField('Article', blank = True)
~~~
+ 활용 당하는 쪽 보다는 활용하는 쪽에 ManyToManyField를 지정하는 것을 권장  
###### 공식 문서  
https://docs.djangoproject.com/en/3.0/ref/models/fields/#manytomanyfield
   
---

### RDBMS지만, DB따라 NoSQL기능도 지원
> ###### 하나의 Post 안에 다수의 댓글 저장 가능
> (NoSQL에서는 테이블이라기보다는 문서를 저장하는 개념)
  
###### djkoch/jsonfield
+ 대개의 DB엔진에서 사용 가능
+ TextField/CharField를 래핑
+ dict 등의 타입에 대한 저장을 직렬화하여 문자열로 저장 
> 내부 필드에 대해 쿼리 불가

###### Django.contrib.postgres.fields.JSONField
+ 내부적으로 postgreSQL의 jsonb 타입
+ 내부 필드에 대해 쿼리 지원
###### adamchainz/django-mysql
+ MySQL 5.7 이상에서 json 필드 지원
---
### 코드 예시
~~~
post = Post.objects.get(title="첫번째 글")

Tag.objects.create(name = '파이썬') #Tag 객체를 추가
Tag.objects.create(name = '장고')
Tag.objects.create(name = '문서')
Tag.objects.all()

tag = Tag.objects.first() # '파이썬'

#역추적 가능, 관계된 post객체들과 tag 객체들을 조회
tag.post_set.all()
post.tag_set.all()

#리스트&튜플과 같은 컨테이너형 변수 단위의 추가도 가능
tag_qs = Tag.objects.all()
post.tag_set.add(*tag_qs) # * unpacking
~~~