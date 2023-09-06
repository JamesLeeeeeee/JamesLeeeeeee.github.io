---
layout: post
title: jupyter notebook 컨테이너로 외부 접속
categories: jupyter notebook 컨테이너 외부접속 generate
description: jupyter notebook 컨테이너로 외부 접속
keywords: jupyter notebook 컨테이너 외부접속 generate
---

>자주 잊어버리는 것 중 하나..
도커 컨테이너 생성할 때 
포트설정 후 다른 설정도 필요하다.

## config파일 생성

~~~
$ jupyter notebook --generate-config
=> 이러면 대략 루트경로 안에 config파일이 생성된다.
~~~


## 비밀번호 설정
~~~
ipython or python

from notebook.auth import passwd
passwd()

Enter password:
Verify password:     # 비밀번호 입력

---------암호화된 비밀번호 등장-----  # 이거 복사
~~~

## config 설정

~~~
$ vi confg경로( 위에서 나온 경로 => 이러면 대략 루트경로 안에 config파일이 생성된다.)

c.NotebookApp.password = 복사한 비밀번호
c.NotebookApp.open_browser = False # 컨테이너안에서는 못여니깐
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.port = 포트번호

~~~

## 실행

~~~
$ jupyter notebook --allow-root --ip localhost
~~~













