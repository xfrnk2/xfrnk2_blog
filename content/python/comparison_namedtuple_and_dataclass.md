---
title: "Python - namedtuple과 dataclass 비교 정리"
date: 2020-11-18T14:10:57+09:00
draft: false

categories: ['Python']
tags: ['Python', 'immutable']
author: "xfrnk2"
---
> ＊namedtuple은 tuple을 상속받았으며 tuple 구조는 c로 작성되었다. 그러므로 해시나 비교 등의 메서드가 빠르다.
  
> ＊dataclass는 dictionary에 기초하므로 장단점의 차이가 생긴다.
  
> ＊예를 들면 공간 사용은 tuple이 더 작으나, 액세스 속도는 dataclass가 더 빠르다.
  
> ＊따라서 원하는 데이터의 구조가 immutable하고, hashable, iterable, unpackable, comparable하다면 namedtuple을 사용하는 편이 좋다. 그리고 예를들어 상속의 가능성 등 복잡한 구조를 원한다면  dataclass를 사용하는 편이 좋다.
  
  
+ 참고 링크  
https://stackoverflow.com/questions/51671699/data-classes-vs-typing-namedtuple-primary-use-cases
