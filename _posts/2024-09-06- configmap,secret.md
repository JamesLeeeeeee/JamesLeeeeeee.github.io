---
layout: post
title: Configmap, Secret
categories: kubernetes Configmap Secret
description: Configmap, Secret
keywords: kubernetes Configmap Secret
---

~~~
이 글은 인프런 쿠버네티스 어나더 클래스 복습자료입니다.
~~~

![image](https://github.com/user-attachments/assets/9b5d5bf9-21ad-4c01-be32-3399f427d4c4)

### Configmap

  spring_profiles_active : 'dev' -> dev 환경에서 app이 돌아감
  application_role : "ALL" -> app의 역할
  postgresql_filepath:~~ -> 외부환경을 app으로 주입시키기 위한 값. 해당 경로는 Pod의 mouthPath에서 정하고, app의 환경변수에서 db정보 확인하고 접속 가능.

### Secret
 postgre-info.yaml 으로 저장
 stringData는 base64로 인코딩되어 저장하고 컨테이너에서는 다시 디코딩 된다.
 Pod 설정 시 volumeMounts에 mouthPath를 설정하면 해당 Path로 설정됨.
 크게 보안에 이점이 없음

![image](https://github.com/user-attachments/assets/f7631585-9843-48b6-a590-6168c99a2a3f)

### 영역 파괴범 configmap

  쿠버네티스 적용을 안하는 프로젝트는 개발자, 데브옵스, VM관리자 모두 환경설정파일을 가지고 있지만, 적용을 하면 configmap에서 관리가 가능하다.
  하지만 의견의 차이나 환경설정파일의 관리 방법이 다르기 때문에 충분한 논의와 설명이 필요하다. 충분히 이해시키도록 설명할 줄 알아야하고 상대방의 방법도 존중해야한다.


### 이름과 다른 Secret
  ![image](https://github.com/user-attachments/assets/31f7a136-2581-4de6-87a8-b5e8b9f220b8)

* secret에는 type설정이 있는데 opaque는 configmap과 거의 동일, 다를게 설정 가능

* docker-registry
  * private한 docker hub를 제공
  * docker-username / docker-password / docker-email key 포함됨
  * pod와 imagePullSecret으로 매핑

> 방안 1
> 클러스터내에서 직접 생성관리
> object를 yaml파일 배포하려면 배포서버를 거쳐야 해서 모든 object를 내부에서 전부 사용 불가.
> 비밀번호를 사용하는 secret만 클러스터에서 직접 관리

> 방안2
> 자체 암호화
> 복호화 로직을 app에 포함시켜야 한다.

> 방안3
> 서드파티 사용
> vault와 같은보안관련 app사용. 요청이 가면 관리자가 지정해서 pod에 비밀번호 설정.


