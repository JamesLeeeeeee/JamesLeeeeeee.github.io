---
layout: post
title: flutter 'List<dynamic>' is not a subtype of type 'List<String>'
categories: flutter, List<dynamic>, 리스트변환, 플러터
description: flutter 리스트 다이나믹을 리스트 스트링으로
keywords: flutter, List<dynamic>, 리스트변환, 플러터, List<String>
---


>플러터 사용 중 타입에러를 자주 볼 수 있는데 한 가지 기술을 배웠다.

## List<type>.from(element)

~~~
'List<dynamic>' is not a subtype of type 'List<String>'
~~~

위와 같이 뜬다면

>String 타입을 받아야한다.

>List<String>.from(element)


ex)
~~~
name: item['tags'] <= 에러

==>
    name: List<String>.from(item['tags'])
~~~














