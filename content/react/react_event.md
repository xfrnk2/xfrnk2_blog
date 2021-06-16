---
title: "React(리액트)_Event(이벤트)"
date: 2020-09-03T23:46:41+09:00
draft: false

categories: ['React']
tags: ['React']
author: "xfrnk2"
---
### 종류
+ onClick
+ onChange
+ onSubmit
  
### 형태?

<태그 on이벤트명 = {(e) => {호출할 함수(e)}}> (e는 event)
~~~
<Fragment>
	<div>
		<form onSubmit={(event)=>{showForm(event)}}>
			<input type="text" name="title" onChange={(e) => {changeForm(e)}}/>
			<input type="text" name="reason" onChange={(e) => {changeForm(e)}}/>

			<button>submit</button>
		</form>
		
		{foods.map((food) => { 
			return <div className="food">{food.title}</div>
		})}
	</div>

</Fragment>
~~~
  
### 이벤트 발생 방지
+ 이벤트 자체를 막는 것. ex) "새로고침"
~~~
const showForm = (event) => {
	event.preventDefault();
	
	axios.post("/posts", form)
		.then((response) => {
			console.log(response);
		})
		.catch((error) => {
			console.log(error);
		});
}
~~~

### Javascript 문법
+ 배열 원소 병합
~~~
arr1 = [1, 2, 3];
arr2 = [4, 5];
~~~
~~~
totalarr = [...arr1, ...arr2]
~~~
+ ...를 앞에 붙이면 언패킹
  
  
+ 배열 원소 병합2
~~~
arr1 = {
	이름 : "김철수",
	나이 : 35
}
arr2 = {
	이름 : "김영희",
	나이 : 32
}
~~~
~~~
arr1
~~~
