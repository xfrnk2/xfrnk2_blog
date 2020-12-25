---
title: "FFT in React_PreventDefault로 알아본 이벤트 핸들링"
date: 2020-12-25T13:34:33+09:00
draft: false

categories: ['Food For Thought']
tags: ['React', 'FFT', 'props']
author: "xfrnk2"
---
> Foot For Thought : 생각할 거리, -를 줄여서 FFT-
  
&nbsp; React(리액트)를 사용하여 TodoList를 구현하던 도중 예기치 않게 발생한 에러를 통해 이벤트 핸들링에 대해 알아보았다.
  
---
### 문제가 발생한 시점
- `<div>` 내부에 `<div>`를 선언하여, 부모 레벨 `div`에서는 `onClick` 이벤트를, 자식 레벨 `div`에서는 form을 사용한 `onSubmit` 이벤트를 발생시키도록 했다.
- 콘솔로 찍어 확인 한 결과 부모 레벨의 `div`의 `onClick` 이벤트만이 계속 나오는 것이 확인되었다.
   
---
### 문제가 있었던 부분
- SPA 방식에 기준하여 페이지의 전체 새로고침을 하지않고 특정 부분만을 갱신시키기 위해 preventDefault()를 남용한 것이 화근이었다.
- 부모 레벨에서 발생시키는 함수 내부의 preventDefault()를 없애니 증상이 해결되었다.
  
---
### 문제를 개선한 모습
부모레벨과 자식레벨에 각각 1, 2를 콘솔창에 찍도록 시험 해 본 결과, 해결 전에는 계속 1이 나왔고, 해결 뒤에는 적절히 1과 2가 나왔다.
  
---
### 비고
`preventDefault()` 말고도 `event.stopPropagation()` 또한 참고가 되었는데 문제 해결에 유효하진 않았던 것으로 보인다.
- mdn의 주의사항에 아래와 같이 명시되어 있다.
>  Calling preventDefault() during any stage of event flow cancels the event, meaning that any default action normally taken by the implementation as a result of the event will not occur.
- 보고 배운 사이트 
> https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/
---
### 추가 사항
+ 탐색 순서가 bottom to top(내부->외부) / 탐색 순서가 top to bottom(외부->내부)
+ 기본적으로 브라우저가 웹페이지 로딩시에 페이지의 요소와 이벤트를 탐색하게 되는데, 이 때 읽어나가는 순서적인 관점에서 `이벤트 버블링`과 `이벤트 캡쳐링`의 개념이 발생하는 것으로 이해했다.
> “Event bubbling and capturing are two ways of event propagation in the HTML DOM API when an event occurs in an element inside another element, and both elements have registered a handler for that event. The event propagation mode determines in which order the elements receive the event.”

