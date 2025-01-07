---
layout: post
title: triton model output 검색
categories: triton model output 검색
description:  triton에서 모델 장착 후 모델 output의 형태 검색색
keywords: triton model output 검색
---

# triton model inspect

~~~
curl localhost:8000/v2/models/yolov11_engine
~~~

triton에 모델을 넣고 input하고 output에 어떤게 있는지 모를 때가 있다.

바로 검색하면 모델의 구조를 결과로 확인 할 수 있다.

![image](https://github.com/user-attachments/assets/d035ea51-82cb-4ade-945f-07aed8633141)