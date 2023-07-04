---
layout: post
title: docker none image 삭제 
categories: docker, noneImage, delete, dangling
description: docker none 이미지 한번에 삭제하기
keywords: docker, noneImage, delete, dangling, rmi
---

## none 이미지 삭제

도커 빌드하기를 여러번 
이미지 조회를 하니 none태그 된 즉 빌드되지 못한 이미지들이 쌓였다.

이미지들을 
>docker rmi id  

하나씩 삭제하기는 굉장히 번거롭다.

![dangling](/images/blog/20230704/dangling.png)  
- 훨씬 많았다.

찾아 보니 빌드가 잘 되지 않은 이미지를 dangling image라고 한다.

~~~
docker rmi $(docker images -f "dangling=true" -q)
~~~
 - 이미지 필터링으로 dangling 이미지 삭제  
~~~
docker rmi -f $(docker images -f "dangling=true" -q)
~~~  
- -f 옵션을 주어 컨테이너에서 사용중인 이미지여도 강제 삭제

~~~
docker image prune
~~~  
- dangling 이미지 삭제 다른버전

### 번외

~~~
docker rm $(docker ps --filter status=exited -q)
~~~  
- exited된 컨테이너 먼저 삭제

~~~
docker container prune
~~~  
- 중지된 컨테이너 모두 삭제

~~~
docker image prune -a
~~~  
- 컨테이너에서 사용하지 않은 이미지들 모두 삭제

~~~
docker volume prune
~~~  
- 컨테이너에서 사용하지 않는 모든 볼륨 삭제

~~~
docker system prune
~~~  
- 컨테이너, 이미지, 볼륨 모두 정리 (-a 옵션은 사용하지 않는 것도 제거)


