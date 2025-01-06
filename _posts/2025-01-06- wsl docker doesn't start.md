---
layout: post
title: wsl docker doesn't start
categories: wsl docker start service ubuntu 22.04
description: 도커가 실행이 안될 때때
keywords: wsl docker start service ubuntu 22.04
---

# wsl에 도커를 설치 후

~~~
service docker start

service docker restart

service docker status
~~~

하니까

Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

이게 뜨고

service docker status  으로 계속 확인하니니

Docker is Running 이 뜨다가

다시 

Docker is not Running 으로 바뀌면서 실행이 안되는 것을 확인했다.


방법은 

~~~
sudo update-alternatives --config iptables
~~~

후에 1번을(iptables-legacy) 선택

~~~
sudo service docker start
~~~

이유 : ubuntu 22.04 는  default가 iptables-nft로 돼 있어서다.

잘된다.

출처 : https://crapts.org/2022/05/15/install-docker-in-wsl2-with-ubuntu-22-04-lts/