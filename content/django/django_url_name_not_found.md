---
title: "Django - NoReverseMatch 에러, Reverse not found 해결방법"
date: 2021-06-04T01:44:11+09:00
draft: false

categories: ['TIL']
tags: ['TIL']
author: "xfrnk2"
---
#### Error
> NoReverseMatch at /polls/
> Reverse for 'detail' not found. 'detail' is not a valid view function or pattern name.
---
`\polls/templates/polls`  

~~~
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
~~~
`'detail'` >> `'polls:detail'`으로 변경.
~~~
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
~~~
---
> polls 앱의 urls.py는 settings.py과 같은 레벨에 위치한 urls.py 파일의 urlpatterns에 포함되어 있기 때문이다.   
~~~
urlpatterns = [
    ...
    path('', include('polls.url', name='polls')
]
~~~
