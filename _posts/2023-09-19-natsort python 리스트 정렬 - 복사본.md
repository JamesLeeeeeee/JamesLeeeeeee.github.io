---
layout: post
title: python list 윈도우 이름 순서로 정렬
categories: python natsort list sort 윈도우순서
description: python list 윈도우 이름 순서로 정렬
keywords: python natsort list sort 윈도우순서
---


> 윈도우의 정렬 순서는 파이썬의 sort 순서와 다르다.
예를 들면 파이썬의 정렬 대로는 파일_1.jpg 다음에 파일_2.jpg가 오길 기대하지만 실제로는 파일_10.jpg가 다음에 올 수 있는 점이다.

~~~
sorted(datalist, key=lambda x: int(x.split('.')[0])) 
~~~
>위 처럼 할 수도 있지만 복잡하고 일단 위의 코드는 '.'을 기준으로 앞에 숫자로 되어있을 때만 가능해서 탈락이다.

## natsort
~~~
pip install natsort
~~~
~~~
import natsort

natsort.natsorted(datalist)
~~~

>natsort를 활용하면 윈도우에서 이름 순서로 적용한 순서를 볼 수 있다.