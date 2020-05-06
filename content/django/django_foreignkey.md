---
title: "Django - 관계를 표현하는 Model Field - Foreign Key"
date: 2020-05-02T23:07:21+09:00
draft: false

categories: ['Django']
tags: ['Django', 'Model']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---

### ForeignKey(**to, on_delete**)
+ 1:N의 관계에서 N측에 명시

###### to
+ 대상모델, 클래스 직접 지정 또는 클래스명을 문자열로 지정
+ 자기 참조는 'self' 지정
###### on_delete : Record 삭제시 Rule
+ CASCADE : 장고 1.x에서의 디폴트값 / 주로 CASCADE를 사용, FK를 참조하는 다른 모델의 Record도 삭제
+ PROTECT : ProtectedError (IntegrityError 상속) 를 발생시키며 삭제 방지 
+ SET_NULL : null로 대체, 필드에 null=True 옵션 필수 
+ SET_DEFAULT : 디폴트 값으로 대체, 필드에 디폴트값 지정 필수 
+ SET : 대체할 값이나 함수를 지정, 함수의 경우 호출하여 리턴값을 사용 
+ DO_NOTHING : 어떠한 액션X, DB에 따라 오류 발생 가능성
---
### 올바른 모델 지정 방법
###### 권장하지 않는 방법의 예시

~~~
#models
from django.contrib.auth.models import User
~~~
~~~ 
post = models.ForeignKey('auth.User', on_delete=models.CASCADE)
~~~

###### 권장하지 않는 이유
+ 적용은 되지만 보장되는 방법은 아님
+ 장고는 모델이 변경될 수 있기 때문에 확실한 방법이 아님
+ 이 유저 모델이 아닌 다른 유저 모델이 현재 장고 프로젝트에 활성화 되어 있을 가능성이 있음
---
###### 권장하는 모델 지정 방법
~~~
# setting.py
AUTH_USER_MODEL = '해당 앱이름.유저모델이름'
~~~
+ 위 값의 디폴트 값은 'auth_User'
+ django.conf.global_settings.py에 저장되어 있고, 항상 현재 활성화 되고 있는 유저 모델을 가리키고 있음을 의미하게 됨
~~~
# models.py
from django.conf import settings
post = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
~~~

+ **가장 안전하고 확실하게 유저 모델을 지정할 수 있는 방법**
+ **프로젝트 만들자 마자 하는 것이 좋음**

---
### 코드 예시
**동일한 코드**
~~~
comment = Comment.objects.all().first()
comment = Comment.objects.first()
~~~
**사용 예시** 
~~~
comment.post  
# Post.objects.get(pk=comment.post_id)
~~~
+ SELECT가 두번 이루어짐 
+ 첫째줄의 comment라는 모델 객체가, 아직 post라는 객체를 만들지 않았다면, 둘째 줄의 동작을 내부적으로 수행, 자동 할당
---
### 코멘트 리스트 가져오기
###### 모두 같은 결과를 도출
~~~
Comment.objects.filter(post_id=4) #직접적인 필드
Comment.objects.filter(post__id=4) #post 측에 있는, 관계에 있는 id
~~~
~~~
Comment.objects.filter(post=post) #추천
post.comment_set.all() #추천, 사용하기 좋은 코드
~~~
---
### 외래키에서의 Reverse name
###### reverse 접근 시의 속성명 : 디폴트 -> "모델명소문자_set"
---
#### **reverse_name의 이름 충돌이 발생한다면?**
  ###### reverse_name 디폴트 명은 앱이름 고려 x, 모델명만 고려
  + 서로 다른 이름의 앱에서 Comment라는 이름의 같은 모델명을 사용할 경우
  ---
  ###### 다음의 경우, user.post_set 이름에 대한 충돌
  + blog 앱 Post 모델 author = FK(User)
  + shop앱 Post 모델 author = FK(User)
 > 2개 이상이며, 이름 충돌이 날 때 makemigrations 실패
 #### **이름 충돌 피하기**
 1. 어느 한 쪽의 FK에 대해, reverse_name을 포기 ->related_name='+'
 + reverse_name을 사용하지 않겠다는 선언
 2. 어느 한 쪽의 (혹은 모두) FK의 reverse_name을 변경
    1. ex) FK(User, ... , related_name = "blog_post_set")
    2. ex) FK(User, ..., related_name = "shop_post_set")
---
## limit_choices_to 옵션

###### Form을 통한 Choice 위젯에서 선택항목 제한 가능.
+ dict/Q 객체를 통한 지정 : 일괄 지정
+ dict/Q 객체를 리턴하는 함수 지정 : 매번 다른 조건 지정 가능

###### ManyToManyField에서도 지원
~~~
staff_member = models.ForeignKey(
User,
on_delete=models.CASCADE,
limit_choices_to={'is_staff': True}, #조건을 걸 수 있음!
)
~~~

**또다른 사용예시**
~~~
limit_choices_to={is_public : True} # 공개여부
~~~

