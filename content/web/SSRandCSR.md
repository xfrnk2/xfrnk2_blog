---
title: "서버 사이드 렌더링(Server Side Rendering: SSR) & 클라이언트 사이드 렌더링(Client Side Rendering: CSR) 정리"
date: 2020-09-26T00:54:03+09:00
draft: false

categories: ['web']
tags: ['web', 'rendering']
author: "xfrnk2"
---
> 최근 용어 '서버 사이드 렌더링(Server Side Rendering)'을 알게 되었는데, 리액트를 공부하며 배우게 된 SPA방식과 관계가 있다고 생각했다.
> 궁금해서 찾아본 내용(서버 사이드 렌더링((Server Side Rendering))과 클라이언트 사이드 렌더링(Client Side Rendering))을 간단하게 정리해 보았다.
---
### Server Side Rendering
  
+ 전통적인 웹 어플리케이션 렌더링 방식
+ 검색엔진 최적화 용이
+ 서버에서 view를 렌더링해서 가져오므로 view를 보기까지 초기 구동속도가 빠름
+ HTML에 모든 컨텐츠가 저장되어 있으므로 검색엔진 노출에 문제가 되지 않음
+ 사용자가 처음으로 보는 컨텐츠의 시점을 앞당길 수 있음
+ 사용자 정보를 서버측에서 세션 관리
---
### Client Side Rendering

+ SPA 방식(Single Page Application)
+ HTML을 다운로드 받고, JS를 다운로드 받아서, JS가 동작하면서 데이터만 주고받아서 클라이언트 쪽에서 렌더링 하는 방식
+ 브라우저가 없으므로 HTML만 가져와서는 검색에 나오지 않음. 웹 크롤러들이 HTML에서만 컨텐츠를 수집하므로 빈 페이지로 인식
+ Googlebot과 SearchConsole에 검색 노출이 안됨
+ 초기 구동속도가 느리지만 이를 제외한다면 빠른 반응을 보여줌
+ 일관성 있는 코드를 작성할 수 있음
+ 클라이언트 측 쿠키말고는 사용자에 대한 정보를 저장할 공간이 마땅치 않음