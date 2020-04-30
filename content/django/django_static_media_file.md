---
title: "Django Static & Media 파일"
date: 2020-04-28T23:25:41+09:00
draft: false

categories: ['Django']
tags: ['Django']
author: "xfrnk2"
---
>이진석 선생님의 [리액트와 함께 장고 시작하기](https://educast.com/course/web/ZU53) 수강중 정리한 글입니다.
---
### Static & Media 파일

+ Static 파일 : 개발 리소스로서의 정적인 파일(js, css, image 등)
+ 앱 또는 프로젝트 단위로 저장 및 서빙
  
---  
### Media 파일 (장고에만 있는)
+ FileField/ImageField를 통해 저장한 모든 파일
+ DB필드에는 저장 경로를 저장하며, 파일은 파일 스토리지에 저장
+ 실제로 문자열을 저장하는 필드(중요) -> 파일은 스토리지에 저장, 경로 문자열이 저장
+ 프로젝트 단위로 저장 및 서빙
  
---
### Media 파일 처리 순서 
  
1. HttpRequest.FILES를 통해 파일이 전달
2. 뷰 로직이나 폼 로직을 통해, 유효성 검증을 수행
3. FileFIeld/ImageField 필드에 "경로(문자열)"를 저장
4. setings.MEDIA_ROOT 경로에 파일을 저장
  
###### 코드 예시
  
~~~
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, '..', 'public', 'media')
~~~
> MEDIA_ROOT의 경우, 상위 디렉토리(..)의 public/media에 저장
---
### File Field
  
+ File Storage API를 통해 파일 저장
+ 장고에서는 FIle System Storage만 지원, django-storages를 통해 확장 지원
+  해당 필드 옵션필드로 두고자 하면 _blank=True 적용
---
### ImageField (File Field 상속)
  
+ Image Field 는 Pillow 이미지 처리 라이브러리 설치를 통해 width와 height 획득
+ 미설치시 ImageField를 추가한 makemigrations 수행에 실패
+ 빈 경로 처리여부 : blank=True의 디폴트 설정은 false
###### 위 필드를 상속 받은 커스텀 필드를 만들 수 있다.
---
### 사용할 만한 필드 옵션
###### 　 blank 
+ 업로드 옵션 처리여부
+ default : False
###### 　 upload_to
+ settings.MEDIA_ROOT 하위에서 저장한 파일명/경로명 결정
+ default : 파일명 그대로 settings.MEDIA_ROOT에 저장
+ 성능을 위해 한 디렉토리에 너무 많은 파일이 저장되지 않도록 조정하는 역할(추천)
+ 동일 파일명으로 저장시에 파일명에 더미 문자열을 붙여 파일 덮어쓰기 방지
###### 　upload_to 주의사항
+ 지정하지 않을 경우 MEDIA_ROOT 밑에 파일을 저장
+ 파일 저장시에 upload_to 인자를 변경한다고 해서, DB에 저장된 경로값이 갱신되지는 않음
###### 　upload_to 인자 유형
+ 문자열로 지정 : 파일을 저장할 "중간 디렉토리 경로"로서 활용
+ 함수로 지정 : "중간 디렉토리 경로" 및 "파일명"까지 결정 가능
---
### 파일 저장 경로 / 커스텀 upload_to 옵션
한 디렉토리에 파일을 너무 많이 몰아둘 경우, OS 파일찾기 성능 저하. 디렉토리 Depth가 깊어지는 것은 성능에 큰 영향 없음. 필드 별로, 다른 디렉토리 저장경로를 가질 것을 권장
  
  
###### 대책 1) 필드 별로 다른 디렉토리에 저장
+ photo = models.ImageField(upload_to="blog")  
+ photo = models.ImageField(upload_to="blog/photo")
   
###### 대책 2) 업로드 시간대 별로 다른 디렉토리에 저장
+ upload_to에서 strftime 포맷팅을 지원 
+ photo = models.ImageField(upload_to="blog/%Y/%m/%d")
---
### uuid를 통한 파일명 정하기 예시
~~~
import os
from uuid import uuid4 
from django.utils import timezone

  
def uuid_name_upload_to(instance, filename):
	app_label = instance.__class__._meta.app_label 	# 앱 별로
	cls_name = instance. __class__.__name__.lower()# 모델 별로
	ymd_path = timezone.now().strftime('%Y/%m/%d') 	# 업로드하는 년/월/일 별로 
	uuid_name = uuid4().hex
	extension = os.path.splitext(filename)[-1].lower() #파일명과 확장자로 분리
	return '/'.join([
			app_label, 
			cls_name, 
			ymd_path, 
			uuid_name[:2], 
			uuid_name + extension,
	])

~~~
###### uuid의 사용
~~~
>>> from uuid import uuid4 
>>> uuid4()
UUID('2456b825-1e40-4da5-a8a8-114afb1b2ec4')
>>> uuid4().hex
'be8a02e5b8bd41c883d09afba142a6c9'
~~~
###### splitext의 사용
~~~
>>> from os.path import splitext
>>> splitext('c:/hello/python.py') # 확장자가 없으면 두번째 원소로 ''를 반환
('c:/hello/python', '.py') # splitext('경로')[-1]과 같은 형태로 사용
~~~
---
### 개발환경에서의 media 파일 서빙
+ static 파일과 다르게, 장고 개발서버에서 서빙 미지원
+ 개발 편의성 목적으로 직접 서빙 Rule 추가 가능
~~~
from django.conf import settings
from django.conf.urls.static import static
# 중략
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
~~~
---
### File Upload Handler
###### 파일 크기가 2.5MB 이하의 경우
+ 메모리에 담겨 전달
+ MemoryFileUploadHandler
###### 파일 크기가 2.5MB 초과일 경우
+ 디스크에 담겨 전달
+ TemporaryFileUploadHandler
###### 관련 설정
+ settings.FILE_UPOLAD_MAX_MEMORY_SIZE
+ -> 2.5MB
---
### 기타 팁
###### ㅇ if settings.DEBUG: in urls.py 
~~~
if settings.DEBUG:
    urlpatterns += static.static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
~~~
+ 파일을 읽어서 응답을 주기 위한 방법 (urls.py)
+ 장고는 미디어 파일등의 스테틱 파일의 서빙을 실제 프로덕션에서 하는 것을 권장하지 않음
+ 만약 if 문을 한 줄 없앤다 해도 빈 리스트를 반환하게 되지만, 명시적으로 사용하기 위해서 if문을 사용

###### ㅇ mark_safe()
+ url로 표시되는것은 django의 feature
+ django.utils.safestring.mark_safe() 가 경로
+ url이 아닌 이미지가 정상적으로 나올수 있게 함
  
###### ㅇ 조회
~~~
>>> a = Post.objects.all()[2]
<Post: 세번째>
>>> a.photo.url
'/media/%EA%B7%B8%EC%9C%BC%EB%9E%98.jpg'
>>> a.photo.path
'C:\\Users\\rad87\\Documents\\programming\\mysite\\django_tech\\이미지.jpg'
~~~

###### ㅇ settings의 import
>settings는 기본적으로 django.conf의 global_settings와, 현재 프로젝트의 settings가 합쳐져야 되므로
>from django.conf import 현 프로젝트의 settings인 settings 로 하면 합쳐짐(오버라이팅)
~~~
# from django.conf import global_settings
# from django_tech import settings
from django.conf import settings
~~~
---
  
### 개인 질문
+ 강사님께 1:1로 질문 드려본 결과 아래와 같은 친절한 답변을 받았다.
###### Q : 
옵션필드라는 것은 무엇인가요?
###### A : 
장고에서는 Form기능을 통해, 유저에게 입력폼을 제공하고, 유저가 입력한 값에 대한 유효성 검사를 수행합니다.
옵션필드라는 말의 의미는, 해당 값에 대해서 값 입력을 요구하지 않겠다는 의미입니다.
blank=True는 문자열 필드에 대해서 적용하구요. 값 입력이 없는 빈 문자열 상황을 허용하겠다는 것입니다.
FileField/ImageField는 CharField를 상속받은 필드입니다. 문자열 특성을 가지고 있다고 볼 수 있습니다. 실제로 db에는 파일 경로 문자열을 저장합니다. blank=True로 지정했다는 것은 파일을 지정하지 않은 상황을 허용하고, 이때 db에는 빈 문자열 경로를 저장합니다.
  
(값이 필수가 아니여도 된다는 의미)
###### + 친절하게 해주신 설명으로 명쾌하게 이해되었다!

