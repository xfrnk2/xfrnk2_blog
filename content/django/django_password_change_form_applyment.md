---
title: "Django - 사용자 정의 템플릿이 PasswordChangeForm의 password_change_form.html에 의해 렌더링 되지 않는 이유와 문제 해결"
date: 2021-06-28T23:44:11+09:00
draft: false

categories: ['Django']
tags: ['Django']
author: "xfrnk2"
---
#### 문제
> 비밀번호 변경 기능을 추가하고 싶었다.  
> 어떻게? Django에서 기본 제공하는 PasswordChangeForm, [`password_change_form.html`](https://github.com/django/django/blob/b9cf764be62e77b4777b3a75ec256f6209a57671/django/contrib/admin/templates/registration/password_change_form.html)을 사용 `< click`    
>  
> 직접 작성한 템플릿에 의해 비밀번호 변경 페이지가 렌더링 되게 하고싶었는데, 
> 원하던 템플릿이 아니라, Django **Administration**에서 제공하는 페이지가 렌더링 되었다. 
---
##### 내가 원했던 화면은 이게 아닌데...?
![K-007](https://user-images.githubusercontent.com/34790699/123660115-55a7e200-d86e-11eb-8f07-5e8f388b9d76.png)
---
#### 관련 실마리
> **[Django 공식 문서의 Installed_apps settings:](https://docs.djangoproject.com/en/3.0/ref/settings/#installed-apps)**  `< click`   
> + **When several applications provide different versions of the same resource (template, static file, management command, translation), the application listed first in INSTALLED_APPS has precedence.**  
> + *번역 : 여러 애플리케이션이 동일한 리소스 (템플릿, 정적 파일, 관리 명령, 번역)의 다른 버전을 제공하는 경우 INSTALLED_APPS에 첫 번째로 나열된 애플리케이션을 우선시 한다.)*
---
#### 해결 방법

> Installed_apps 내에서 admin보다 나의 애플리케이션을 우선 오게 만드니 해결되었다!

**Before**
~~~
INSTALLED_APPS = [
    # Default App
    'django.contrib.auth',
    'django.contrib.admin',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # My App
    'blog'
	...
]
~~~
---
**After**
~~~
INSTALLED_APPS = [

    # My App
    'blog'
	...

    # Default App
    'django.contrib.auth',
    'django.contrib.admin',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    

]
~~~
---
##### 원했던 화면, 직접 작성한 템플릿을 통해 정상적으로 출력된다.
![K-008](https://user-images.githubusercontent.com/34790699/123660129-58a2d280-d86e-11eb-8c6a-28463847f875.png) 

#### [알게된 곳](https://stackoverflow.com/questions/58496482/django-custom-template-is-not-rendering-for-password-change-form-html) 

