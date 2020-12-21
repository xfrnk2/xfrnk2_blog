---
title: "FFT in React_props 서빙시 반드시 {}괄호를 사용하자!"
date: 2020-12-21T13:34:33+09:00
draft: false

categories: ['Food For Thought']
tags: ['React', 'FFT', 'props']
author: "xfrnk2"
---
> Foot For Thought : 생각할 거리, -를 줄여서 FFT-
  
&nbsp; React(리액트)를 사용하여 Todo List를 만들었는데, 최종적으로 만들기 직전에 겪었던 문제에 대해서 간단히 정리하기 위해서 글을 쓴다.
  
---
### 문제가 발생한 시점
Todo List로부터 Todo 데이터의 `{todo.title}`(제목)과 `{todo.content}`(내용)을 출력하려고 했는데, `undefined` 문구가 나오면서 출력할 수 없었다.
   
---
### 문제가 있었던 부분
Todo 컴포넌트 선언시 props를 받아오는 부분에서 중괄호를 사용하지 않았던 것이 문제점이였다.
~~~
const Todo = (todo, todos, setTodos) => {.....}
~~~
  
---
### 문제를 개선한 모습
~~~
const Todo = ({todo, todos, setTodos}) => {.....}
~~~
  
---
### 비고
크롬의 개발자 도구를 이용해서 console.log()를 계속해서 찍어보았는데, 왜 자꾸 todo.title, todo.content가 아닌, 중괄호로 감싸진 날것의 todo 컴포넌트의 전체 데이터가 나오는지 원인을 알 수 없었다.  
하나하나 확인해 본 결과 input으로 받아오는 props의 형태를 바깥에서 중괄호({})를 감싸지 않았던 것이 문제였음을 알 수 있었고, 좋은 발견이 되었던 것 같아 글을 남긴다.  
  
---
### 추가 사항
덧붙이자면 {} 중괄호를 감싸지 않고 `(todo, todos, setTodos)`와 같은 형태로 받아오면 todo에 todo 컴포넌트의 전체 데이터가 담겼고, todos와 setTodos는 `undefined`로 표시됨을 크롬의 개발자 도구를 통해서 확인할 수 있었다.