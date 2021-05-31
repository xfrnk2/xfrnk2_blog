---
title: "Shell script에 권한을 주기 // Error message - make : command not found, Permission denied for linuxwindowsscript.sh file"
date: 2021-02-17T12:39:03+09:00
draft: false

categories: ['resolved problem']
tags: ['resolved problem']
author: "xfrnk2"
---
### Shell script에 권한을 주기

##### makefile의 한 부분

```
OsConf= ./LinuxWindowsScript.sh
# (OS의 종류에 따라 다르게 동작하도록 하는 쉘 스크립트 파일)
```
#### travis-ci Error log
```
./LinuxWindowsScript.sh
make: ./LinuxWindowsScript.sh: Command not found
Makefile:53: recipe for target 'lint' failed
make: *** [lint] Error 127
The command "make lint" exited with 2.
```

#### 해결한 방법
```
git update-index --add --chmod=+x linuxshellscript.sh
git commit -m 'Make linuxshellscript.sh executable'
git push
```
+ 문제 원인 : script doesn't have execute permissions
+ Powershell 사용해서 커맨드 실행 (Windows)
+ 후 정상 작동을 확인

　   
　
  
    
참고한 곳  
https://stackoverflow.com/questions/42154912/permission-denied-for-build-sh-file
