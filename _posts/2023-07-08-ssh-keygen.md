---
layout: post
title: 리눅스 ssh-keygen로 ssh접속
categories: 리눅스, 우분투, linux, ssh, sshfs, publickey,privatekey, ssh-keygen, port, /etc/fstab, 자동마운트, 부팅시
description: 리눅스 ssh-keygen로 ssh접속
keywords: 리눅스, 우분투, linux, ssh, sshfs, publickey,privatekey, ssh-keygen, port, /etc/fstab, 자동마운트, 부팅시
---

![](/images/blog/20230707/   )


>서버가 꺼지는 일이 있어서 nas를 자동으로 마운트시켜주는 걸 설정했었으나
자동으로 되지가 않았다.. 그래서 이번에 손을 봤다.

## ssh-keygen
먼저 sshfs를 사용해서 마운트를 하려고 하는데  
비밀번호없이 입력되게 하고 싶어서 ssh-key를 입력하고자 했다.  
~~~
$ cd ~
$ ssh-keygen -t rsa
~~~

![keygen](/images/blog/20230707/ssh-keygen.png)

입력하면 경로를 설정하라고하는데 엔터를 누르면 루트의 .ssh 경로에 저장된다. 바로 엔터

![keypw](/images/blog/20230707/password.png)  
비밀번호를 입력하라고 나오는데 ssh키 인증만해도 충분하니 엔터 두번

![keysaved](/images/blog/20230707/key_saved.png)  
하면 저장이 됐다고 뜹니다.

![keyls](/images/blog/20230707/savedLs.png)  
저장된 경로에 가면 확인 가능합니다.

![keyCopy](/images/blog/20230707/keycopy.png)  
~~~
ssh-copy-id -i key경로 -p 포트번호 nasId@nasIp
~~~  
를 입력하면 나스아이디의 비밀번호를 입력하라고 나옵니다.

비밀번호를 입력하면 나스경로에 ssh로 입력해보라고 나오는데

비밀번호 없이 들어가지면 성공~

![fstab](/images/blog/20230707/etcfstab.png)  
~~~
nasId@nasIp:/nas경로 /로컬경로 fuse.sshfs port=nas포트,IdentityFile=ssh-key경로,
 allow_other(일반유저접근 허용), uid,gid=1999(user id => id user로 확인 가능),
 _netdev(마운트 전 네트워크 확인),
 delay_connect(접근 가능한 후 디렉토리연결), defualts(나머지 기본 값) 0 0
 (마지막 두 숫자는 파일시스템 dump여부, 리부팅 시 검사여부)
~~~
~~~
$ sudo vi /etc/fstab 에 들어가면 위와같이 입력을 할 수 있는데,
위의 입력은 모두 한줄이다.
port를 잊지말고 꼭 입력해 줍시다.

- 저장
esc
$ wq
~~~

~~~
$ df -h 로 마운트 상황을 보고

mount 되어있다면
$ sudo fusermount -u 마운트해제 경로

다음
$ sudo mount -all
로 /etc/fstab에 쓴 것을 적용 시킨다.

다시
$ df -h 로 마운트 된 것을 확인하면 
-완-
~~~














