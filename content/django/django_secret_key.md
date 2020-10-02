---
title: "Django SECRET_KEY 분리방법"
date: 2020-10-02T18:22:52+09:00
draft: false

categories: ['Django']
tags: ['Django']
author: "xfrnk2"
---
방법은 두 종류가 있는데, 환경 변수로 저장하거나, 파일에 따로 저장하는 방법이다.  
환경 변수로 저장시 아파치 등 웹 서버를 사용하는 경우 저장되지 않을 수 있으므로 후자의 방법을 선택했다.  
임의의 원하는 공간에 secrets.json 파일을 작성 한 뒤, 아래와 같이 입력하고 저장한다.  
딕셔너리 내에서 콤마는 후술(가장 뒤에 덧붙이지)하지 않는다.  
  
**secrets.json**  
~~~
{
  "SECRET_KEY": "실제 SECRET_KEY"
} 
~~~
---
그 다음 setting.py를 아래와 같이 변경한다.
  
**settings.py**
~~~
import os, json
from django.core.exceptions import ImproperlyConfigured

secret_file = os.path.join(BASE_DIR, 'config', 'secrets.json') # 파일 위치

with open(secret_file) as f:
    secrets = json.loads(f.read())

def get_secret(setting, secrets=secrets):
    """변수를 가져오거나 명시적 예외를 반환"""
    try:
        return secrets[setting]
    except KeyError:
        error_msg = "Set the {} environment variable".format(setting)
        raise ImproperlyConfigured(error_msg)

SECRET_KEY = get_secret("SECRET_KEY")
~~~
---
마지막으로 버전관리 시스템(현재 Git)에 secrets.json 파일이 저장되지 않도록 SECRET_KEY를 공개하지 하기 위해 .gitignore 파일 안에 해당 json파일을 추가하면 되겠다.