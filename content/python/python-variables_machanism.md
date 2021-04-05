---
title: "Python - Python의 변수 할당 매커니즘"
date: 2021-04-05T11:19:34+09:00
draft: false

categories: ['Python']
tags: ['Python']
author: "xfrnk2"
---
+ 파이썬은 변수를 생성할 때 변수를 위한 메모리를 생성하는 방식이 아니라, 변수가 만들어지고 어떠한 값을 넣으면 변수가 그 값을 참조하는 형태이다.
+ 변수에 상수 값을 할당하고자 해도 값이 변경되기 때문에 파이썬에서는 상수를 사용하지 않는다. 그 대신 클래스를 사용한 여러가지 방법으로 상수를 정의할 수는 있다.(FrozenSpace, SpaceFrozenValues, `__slots__`, property, metaclass 등, 또는 Python 3.8~ typing.Final(*그러나 재할당 가능))
---
~~~
from uuid import uuid4

    class obj:
        def __init__(self):
            self.__data = uuid4()
            self.__is_changed = False
        @property
        def data(self):
            return self.__data

        def update(self):
            self.__is_changed = not self.__is_changed

        @property
        def is_changed(self):
            return self.__is_changed
			
    group = []
    c = obj()
	
    new_c = c
	new_cc = c
	
    group.append(c) # c를 각각 두 군데, new_c와, 리스트인 group의 0번째 index에 할당한다.

    print('-----------------------구분선------------------------')
	
    print(bool(c == new_c)) # 내부 값을 바꾸기 전 서로 같은지 출력
    
	group[0].update() # group에 속해있는 object의 self.__is_changed의 상태를 True로 변경한다.
    
	print(bool(c == new_c)) # 한쪽만 내부 값을 바꾼 후에 서로 같은지 출력
    
	print(group[0].data) # id를 보여준다.
    
	print(new_c.data) # id를 보여준다.
	
	print(new_cc.data) # id를 보여준다.
    
	print(group[0].is_changed) # self.__is_changed의 상태를 리턴한다.
    
	print(new_c.is_changed)
	print(new_cc.is_changed)
~~~
+ 결론 : 같은 곳을 가리키고 있다.
---
~~~
+ a = 100
b = a
c = a
a = 30
print(a, b, c) // a = 30, b = 100, c = 100
~~~

+ 상수의 경우 a, b, c의 세 변수가 있다고 가정할때, a 변수에 상수값을 할당하고 이 a를 b와 c가 참조하게 할 경우 a 값이 바뀔시 b와 c는 바뀌지 않는다.

---
### 정리 
+ 객체 내부의 값을 위와 같은 구조로 변경시킬 경우, 참조하고 있는 그대로 가리키게 된다.(new_c와 new_cc가 a값을 모두 가리킴)
+ 반대로, a, b, c의 세 변수가 있다고 가정할때, a 변수에 상수값을 할당하고 이 a를 b와 c가 참조하게 할 경우 a 값이 바뀔시 b와 c는 바뀌지 않는다.

