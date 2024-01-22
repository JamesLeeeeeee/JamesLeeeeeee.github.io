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
~~~python
import tika
from tika import parser
data = parser.from_file('/mnt/llm/t/test.pdf')
data['metadata']
~~~
![meta](/images/blog/20240122/2.png)

>위는 메타 데이터를 출력.
>생성날짜, 장당 문자열 수 등


~~~python
data['content']
~~~
![dataContent](/images/blog/20240122/3.png)

>한 덩어리의 텍스트


~~~python
print(data['content'])
~~~
![printContent](/images/blog/20240122/4.png)

>텍스트를 프린트함.


![view](/images/blog/20240122/5.png)

>제 제제제 
>2222
>장장장장
>
>은... 위와 같이 표지일 경우에 많이 나타남.

>결론
>> - 전체 문서를 구조화 된 텍스트로 잘가져옴.
>> - 세로로 되어있는 건 세로로 그대로 들고 옴.
>> - 사람이 보기에는 테이블을 나름 잘 표현 했지만 vectordb에서 찾을 때 잘 못 찾는 경향이 있음
>> - page마다 띄어쓰기가 있어 문서를 나눌 때 이용하면 될 듯.

