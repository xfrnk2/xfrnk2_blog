---
title: "Docker를 Windows와 Linux에서 사용할때 각각 다른게 있을까? <Docker 사용시 Shellscript를 통해 환경변수를 export 하기>"
date: 2020-07-29T23:45:21+09:00
draft: false

categories: ['수습한 삽질기록']
tags: ['삽질']
author: "xfrnk2"
---
&nbsp;Travis-ci에서 python venv 실행시 Linux와 Windows의 activate.bat 파일의 위치가 다른 것을 판단하여 각 운영체제에 맞는 경로의 activate.bat 파일을 실행 할 수 있도록 Makefile을 수정했던 것을 착안해서,  이번에는 Docker에서 Dockerfile을 작성 할 때 마찬가지로 python의 경로를 다르게 할 수 있지 않을까? 하는 생각이 들었다.   
  
&nbsp;Windows는 나의 지정 경로에 python이 설치 되어 있었고, 내가 사용하지 않는 Linux에서는 python의 설치된 경로가 'usr/local/python' 인 것으로 알게 되었기 때문이다. 그래서 환경 변수를 dockerfile에서 어떻게 다루는가에 대해 알아보게 됬는데...  

&nbsp;결론부터 말하면 Docker는 '리눅스 커널에서 제공하는 컨테이너 기술을 이용', 리눅스 이기 때문에 Windows와 Linux를 가를 이유가 없었다.  
  
그래도 삽질한 만큼의 수확은 있었는데, 간단히나마 알게 된 점을 정리해보면..  


> **쉘 스크립트를 사용하여 환경 변수를 export하고 dockerfile에서 사용할 수 있는지?**  
+ 스크립트로부터의 변수를 하위 작업 이미지로 export할 수 있는 방법이 없다.
+ 일반적인 방법으로는 환경 변수를 상위로 올릴 수 없다.
+ ENV로 선언한 환경변수가 개발 환경과 하위 이미지와 컨테이너에서 유지되기 때문에 ENV의 사용을 추천하며, 컨테이너를 실행하거나 exec command를 사용했을 때, Docker는 ENV를 체크하고 현재 실행중인 프로세스의 모든 값을 수송한다.
+ 대안으로서 wrapper file의 사용.(https://github.com/moby/moby/issues/29110)
  
　   
　
  
    
참고한 곳  
https://stackoverflow.com/questions/34911622/dockerfile-set-env-to-result-of-command  
https://stackoverflow.com/questions/47211850/docker-run-script-which-exports-env-variables

