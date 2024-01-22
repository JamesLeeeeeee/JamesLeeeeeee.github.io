---
layout: post
title: tika-python read the pdf
categories: tika tika-python pdf
description: tika-python read the pdf
keywords: tika tika-python pdf
---

# read pdf
llm을 이용해 pdf를 인식 중에 인식을 잘 못하는 것 같아서
하나하나 사용해 보는 중이다.

## tika-python
~~~
https://github.com/chrismattmann/tika-python
~~~


# 설치
java를 통해 실행 되므로 자바를 설치 후에 아래와 같이 설정함.
~~~
!pip install tika
~~~
![gitInfo](/images/blog/20240122/1.png)


# 실행
~~~
import tika
from tika import parser
data = parser.from_file('/mnt/llm/t/test.pdf')
data['metadata']
~~~
![meta](/images/blog/20240122/2.png)
'''
위는 메타 데이터를 출력.
생성날짜, 장당 문자열 수 등
'''

~~~
data['content']
~~~
![dataContent](/images/blog/20240122/3.png)
'''
한 덩어리의 텍스트
'''

~~~
print(data['content'])
~~~
![printContent](/images/blog/20240122/4.png)
'''
텍스트를 프린트함.
'''

![view](/images/blog/20240122/5.png)
'''
제 제제제 
2222
장장장장

은... 위와 같이 표지일 경우에 많이 나타남.
'''