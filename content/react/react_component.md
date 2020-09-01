---
title: "React(리액트)_component 작성시 props와 javascript의 활용"
date: 2020-09-02T00:08:49+09:00
draft: false

categories: ['React']
tags: ['React']
author: "xfrnk2"
---
### 컴포넌트란?
+ 부품
+ 재사용 가능
+ 일일히 바꾸어 줄 필요가 없도록 하기 위함
+ 일명 리액트 컴포넌트라고 부른다.
    
### props란?
+ 자식 컴포넌트가 부모 컴포넌트에게 전달받은 데이터
---
### javascript의 이상하고도 유용한 문법
~~~
let sweats = [50, 30, 100];
let apple = sweets[0];
let banana = sweets[1];
let berry = sweets[2];
~~~
위의 문법을  

~~~
let [apple, banana ,berry] = sweets;
~~~
위처럼 쓸 수 있다.(이상하고 오묘하다)

  
### 훅스(Hooks): useState  
~~~
import {usestate} from 'react';
[state, setstate] = usestate("김철수")
~~~
+ state는 변수, setstate는 함수이다.
+ state = "김덕배"와 setstate("김만춘")를 한다고 하자. 전자는 랜더링이 내장되지 않아 있고 값만 바꾸게 된다. 후자는 렌더링이 내장되어 있어 리랜더링을 수행하고 바로 변경상황을 사용자에게 보여준다.
  
### 훅스(HOoks) : useEffect
~~~
import {useEffect} from 'react';
useEffect(fn, []) 
~~~ 
+ fn는 함수, []안에는 변수가 온다.
+ 속해있는 함수가 호출되는 fn(함수)를 실행하고 싶을때 []를 빈칸으로 둘 수 있다.
+ 뒤에 있는 인자가 바뀔 때마다 fn을 실행한다. 최초 호출시 실행한다.

### 훅스(HOoks) : useEffect
~~~
<Fragment>
	<div>
		<button onClick={changeName}>바꿔</button>
		{name}
	</div>
	<div>
		{num}
		<button onClick={AddNum}>+</button>
		<button onClick={MinNum}>-</button>
	</div>
</Fragment>
~~~
+ 그냥 있는 그대로 퍼왔다. jsx를 쓸 땐 최상위 부모가 하나여야만 한다. return 안에. 현재는 최상위 부모가 div인데 Fragment를 사용해서 감싸둔 상태이다.  
+ 1개 이상 쓰고 싶을때 <Fragment>를 선언하여 코드를 감싸면 된다.

### 번외 - javascript의 map
~~~
return (
	<div>
		Parent
		
		{persons.map((person) => {
			return <Child person={person}></Child>
		})}
	</div>
)
~~~
+ 일부를 가져온 셈 치자.
+ javascript에서는 for문은 상당한 구식이라고 한다. 순회시 map을 주로 쓴다.
+ persons가 배열이고, (person)은 인자를 나타낸다. Child는 person을 인자로 받고, (person)내부의 person이 전달된다.