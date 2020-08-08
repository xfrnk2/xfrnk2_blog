---
title: "Sendgrid Error - “smtplib.SMTPServerDisconnected: Connection unexpectedly closed”"
date: 2020-08-08T23:25:41+09:00
draft: false

categories: ['resolved problem']
tags: ['resolved problem']
author: "xfrnk2"
---
### 발생한 에러
https://sendgrid.com/docs/for-developers/sending-email/django/
1. 위 링크의 문서를 따라서 sendgrid를 사용해 전체 메일을 보낼수 있게 만들고 싶었다.

가상 환경을 실행시켜 환경변수로서 SENDGRID_API_KEY을 선언한 후, 아래 코드의 첫번째 줄에서 호출될 수 있도록 했다.
~~~
SENDGRID_API_KEY = os.getenv('SENDGRID_API_KEY')

EMAIL_HOST = 'smtp.sendgrid.net'
EMAIL_HOST_USER = 'apikey' # this is exactly the value 'apikey'
EMAIL_HOST_PASSWORD = SENDGRID_API_KEY
EMAIL_PORT = 587
EMAIL_USE_TLS = True
~~~

2. 전체메일을 보내기 위해서 커맨드를 입력했는데 아래와 같은 에러가 났다.

~~~
>>> from django.core.mail import send_mail
>>> send_mail('Subject here', 'Here is the message.', 'from@example.com', ['to@example.com'], fail_silently=False)
~~~

> invalid outgoing mail server or port” followed by “smtplib.SMTPServerDisconnected: Connection unexpectedly closed”.

### 해결 방법
sendgrid 홈페이지에서 로그인시 최초에 나오는 왼편의 사이드바- Settings > Account details와 API Key의 세부 내용에 대한 설정을 하고 나니 제대로 이메일이 가는 것이 확인 되었다.

### 알게된 점
+ venv의 환경변수는 activate.bat 파일 내에서 선언할 수 있다.
+ 위의 과정에서 내가 환경변수로서 사용하고자했던 APIkey를 담은 변수 SENDGRID_API_KEY 경우, 아래와 같이 선언하니 정상적 적용되었다. 덧붙여서 :END 라인 이전 줄의 set 커맨드와 같은 라인에 끼워 넣었다. 
~~~
set SENDGRID_API_KEY=APIKEY번호
~~~
+ 환경변수 호출시 Linux에서는 ($variable)이지만, Windows에서는 %variable%이다.

### 도움이 된 곳
+ https://stackoverflow.com/questions/46566536/how-can-you-set-environment-variables-in-a-virtualenv-in-windows
+ https://discuss.erpnext.com/t/sendgrid-smtp-email/58865/6