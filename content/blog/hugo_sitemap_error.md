---
title: "github blog, hugo sitemap couldn't fetch error / 깃허브 블로그, 휴고 사이트맵 가져올수 없음 에러"
date: 2021-06-16T02:02:16+09:00
draft: false

categories: ['blog']
tags: ['blog', 'hugo']
author: "xfrnk2"
featuredImage: "https://user-images.githubusercontent.com/34790699/122094703-68babb00-ce47-11eb-83cf-66e69889ba61.png"
---
+ 그동안 기록해 온 글을 구글에 노출시키기 위해 Google search console에 사이트맵을 등록했는데, 제대로 반영되지 않았다.  
+ hugo 블로그는 원래 sitemap.xml 파일 생성 기능이 있어서, public 폴더 내에 있는 파일을 그대로 이용하면 된다.  
  
+ **문제** : sitemap.xml이라고 추가했는데 불구하고, 가져올 수 없음(couldn't fetch, invalid)인 채로 열흘 지속됨.
+ **해결** : 뒤의 확장자(.xml)을 제외하고 sitemap을 입력하여 제출했더니 곧바로 성공 초록 문구가 나타나며 해결.