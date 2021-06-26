---
title: "Django - takes 1 positional argument but 2 were given 해결"
date: 2021-06-26T01:44:11+09:00
draft: false

categories: ['Django']
tags: ['Django']
author: "xfrnk2"
---
#### Error
> TypeError at /photo/   
> _add_thumb() takes 1 positional argument but 2 were given  

---
`blog/photo/fields.py`의  
`class ThumbnailImageFieldFile(ImageFieldFile):` 내부 함수
~~~
    def _add_thumb(s):
~~~

#### `def _add_thumb(s):'` >> `def _add_thumb(self, s):`으로 변경.  


---

## 에러 발생의 이유  
> That message has umpteen causes; the specific reason here is that all instance methods expect a first arg which by custom we call self. So declaring def method(arg): is wrong for a method, it should be def method(self, arg):. When the method dispatch tries to call method(arg): and match up two parameters self, arg against it, you get that error.  
~~~
    def _add_thumb(self, s):
~~~
> 책의 코드가 반드시 옳은 것은 아니구나. 어디서 문제가 있었는지 확인 & 해결되었다.
> 참고 원글 : [stackoverflow `(click)`](https://stackoverflow.com/questions/23944657/typeerror-method-takes-1-positional-argument-but-2-were-given)