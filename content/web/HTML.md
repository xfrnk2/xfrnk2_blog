---
title: "HTML 태그 요약"
date: 2020-10-05T00:54:03+09:00
draft: false

categories: ['web']
tags: ['web', 'HTML']
author: "xfrnk2"
---

> [비전공자를 위한 HTML/CSS](https://www.edwith.org/boostcourse-cs-htmlcss/lecture/92971)를 보고 HTML 파트를 정리했다. 정의나 유래 등 기초적인 내용에 대해서는 언급하지 않는다. 
---
### 개요
+ HTML : Hyper Text Markup Language
+ 2020/10/2 기준 태그 수 약 130개 정도
+ 실제 자주 사용되는 태그는 25개 정도  
-> 필요할 때 찾고 다 외울 필요는 없다.
  
---
### 빈 태그
+ 내용이 없어서 종료 태그가 필요 없음
+ 화면 표시 등과 다른 용도로 사용되며, 대표적인 경우 브라우저가 직접 화면에 그려줘야 하는 경우다.->대체태그(replacement태그)
+ `<br>`, `<img src="">`
  
---
### 태그와 요소의 차이
~~~
<h1>Hello</h1>
~~~
+ 태그 : `<h1>`과 `</h1>`
+ 요소 : 내용까지 통틀어서 전체
   
---
### 큰 범위에서의 태그의 역할
+ head : 문서의 기본 설정 및 외부스타일 시트 또는 js파일을 연결하는 것
+ meta : 문자의 인코딩 방식 지정하는 것 -> meta charset="UTF-8"
---
  
### 제목과 단락
+ 제목 : heading -> `<h1>`, `<h2>` ...
+ 단락 : paragraph -> `<p>` // 자동개행됨.
+ 강제 개행 : `<br>`
  
---
### 텍스트를 꾸며주는 태그
+ `<b>` bold
+ `<i>` italic
+ `<u>` underline
+ `<s>` strike  
  
---
### 앵커 태그(링크 생성)
~~~
<a href="http:://www.google.com" target="_blank">
~~~

+ href : 주소
+ target : "_self" -> 현재화면에 리소스 표시  
"_blank" -> 새로운 창에 표시  
프레임 딴에서만 사용 -> "_parent", "_top" -> 요즘 안씀
+ 내부 링크 설정시 id를 작성하고 a href란에 "#~~"로 작성  
`<a href="#some-element-id">` -> 주로 최상위로 이동 버튼 구현시 사용
  
---
### 목적에 맞는 사용옹도로 의미가 없는 태그
+ `<div>` -> block-level -> 한줄 생성 후 표현
+ `<span>` -> inline-level -> 한줄 안에 표현
> 컨테이너 요소, 데이터 묶음
  
---
### 리스트 요소
+ `<ul>` unordered list : 번호 없는 것
+ `<ol>` ordered list : 1,2,3 번호 있는 것
+ `<dl>`과 `<dt>`, `<dd>` : 사전처럼 이름, 설명(설명 여러개 가능) 식
  
---
### 이미지 태그 
+ `<img ...>`, 닫는 태그 X
+ src : 이미지 경로
+ alt : 대체 텍스트, 중요시 되는 편
+ width, height : 어느 한쪽만 선언시 자동 비율 대비 크기 조정, 둘 다면 적용값을 따름, 없으면 원본크기
> jpg : 압축률 높고 자연스러운 색상 표현 가능, 투명 미지원, png : 투명 지원, gif : 제한된 색상, 용량 적음

---
### 테이블 태그
+ `<table>`
+ `<tr>`로 행, `<th>`로 값(제목 셀), `<td>`로 값(셀)
+ `<caption>` : 제목
+ `<thead>` 최상단, `<tfoot>` 최하단
+ 세로 병합 -> rowspan="숫자"
+ 가로 병합 -> colspan="숫자"
   
---
### 폼 태그
+ type : "text", "password", "radio", "checkbox"
+ name : 이름
+ placeholder : 배경안내문구
+ type2 : "file", "submit" : 전송, "reset" : 초기화, "image" : 이미지 삽입, src와 alt 필요하며 submit과 동일, "button" : 기능없음

---
### 폼 요소의 종류(하위 태그)
+ `<select>` 내부에 `<option>` 사용
+ 여러줄의 텍스트 입력 받기 -> `<textarea>`
+ `<button type=submit|reset|button>` 텍스트 `</button>`
   
---
### 폼 요소의 종류2(하위테그)
+ `<label>` : form 요소의 이름과 form 요소를 명시적으로 연결 -> 속성 for=id
+ `<fieldset>`, `<legend>`(fieldset의 자식) : 구조화 담당

---
### 큰 폼 태그
+ action -> 서버주소
+ method -> 전송방식 -> get&post (데이터 노출여부)

---
### 비고
+ 코드 작성시 더욱 명시적인 시멘틱 마크업 지향
+ 시멘틱 마크업이란 평범하고 오래된 의미론적 마크업  
POSH(plain old semantic HTML)이라고도 부른다 : 규약과 같다
+ 새로운 HTML5 요소 및 태그 또한 참여
+ Block level : 한 줄
+ Inline level : 한 스코프

