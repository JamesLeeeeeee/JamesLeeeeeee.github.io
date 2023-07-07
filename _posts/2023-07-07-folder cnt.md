---
layout: post
title: 리눅스 우분투 파일 개수
categories: 리눅스, 우분투, linux, filecount, 파일개수, wc
description: 리눅스 파일개수 출력
keywords: 리눅스, 우분투, linux, filecount, 파일개수
---

>자주 잊어버리는 파일 개수 커맨드... 정리해보자

## 현재 위치 파일 개수
~~~
ls -l | grep ^- | wc -l
~~~

## depth 설정으로 하위폴더 개수
~~~
find . -maxdepth 4(depth설정) -type f | wc -l
~~~

## 이름 설정으로 하위폴더 개수
~~~
find . -maxdepth 4(depth설정) -name "*.png" -type f | wc -l

- png 형식의 파일
~~~

## 하위폴더까지 파일개수
~~~
find . -type f | wc -l
~~~











