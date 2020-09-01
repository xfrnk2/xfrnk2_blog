---
title: "React(리액트)_에 대해서"
date: 2020-09-01T23:46:41+09:00
draft: false

categories: ['React']
tags: ['React']
author: "xfrnk2"
---
### 리액트란?
+ 리액트는 페이스북에서 사용되는 웹 프론트엔드 프레임워크이다.  
+ 웹 프론트엔드 프레임워크는 라이브러리가 잘 되어 있어서 컴퓨터는 기반 개발이 용이하다. 또한 SPA방식으로 구현이 편리하기때문에 사용을 권장한다.
+ 웹 프론트 엔드 프레임워크는 대표적으로 리액트, 뷰, 앵귤러가 있다.
+ 뷰는 리액트보다 간결한 편이나, 대규모 프로젝트에서는 뷰보다 리액트를 더 선호하는 편이다. (뷰 < 리액트)
+ 앵귤러는 내용(알아야 할 거, 배워야 할 것)이 아주 많다고 한다.(리액트 < 앵귤러)
+ 요구되는 개발환경은 node-js이다.

### SPA방식이란?
+ 싱글 페이지 방식(Single Page Application)이다.
+ 멀티 페이지 방식(Multi Page Application)보다 가볍다.
+ 대체로 성능이 모바일 < 컴퓨터이기 때문에 모바일에서는 SPA방식을 더 선호한다.

### 리액트 시작은?
~~~
npx create-react-app react-course
~~~
+ react-course에는 원하는 이름을 사용해도 무방하다.

### 리액트 역할은?
+ jsx 파일에 작성한다.
+ jsx파일에서는 자바스크립트 베이스로 'import React from 'react'로 react를 불러와서 사용할 수 있다.
+ jsx -> js파일로 바꿔주는 과정에 node-js가 필요하므로 node-js의 설치를 요구한다.
+ javascript와 react 라이브러리(편리한 함수들)로 비교적 간편하게 작성한 리액트 컴포넌트들(jsx)을 node-js에서 순수 자바스크립트로 바로 변환해준다.
 
### npm과 npx의 차이?
+ npx는 node package management이다.
+ npm은 프로젝트 중심 작업을 관리하는데 사용하고, npx는 단발성(휘발성)작업을 하는데 사용한다. 즉 npm을 사용해서 package.json의 개발시 요구 환경(requirements)인 라이브러리들을 설치할 시 'npm install ~'을 사용한다. node-modules는 gitinore에 추가하기 때문에 현재와 다른 환경에서 개발할 경우 개발환경 셋팅을 위해 package.json에 기록된 개발 요구 환경을 설치할 필요가 생긴다.
+ npx는 프로젝트 시작 'npx create-react-app react-course' 처럼 기타 의존성의 필요 없이 바로 그 순감에만 사용하고 싶을 때 쓰인다.
+ 즉 npm은 패키지 관리-설치, npx는 따로 설치 안하고 바로 이용하기 위해 사용한다.

>Tip.
>예전 자바스크립트에서는 state 제어하는 것을 함수형, state 제어 안하는 것을 클래스형이라고 구분짓고 변수 변경시 바로 리랜더링 하는것을 state 제어한다고 해 왔으나, 지금은 훅스가 나와서 아무래도 괜찮다. 함수형(state 제어)+훅스가 기본이 되었다.


