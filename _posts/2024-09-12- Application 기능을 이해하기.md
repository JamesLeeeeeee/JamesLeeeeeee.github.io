---
layout: post
title: Application 기능을 이해하기
categories: kubernetes PVC, PV Deployment HPA Service
description: Application 기능을 이해하기
keywords: kubernetes PVC, PV Deployment HPA Service
---

kubernetes Application 기능을 이해하기
PVC, PV Deployment HPA Service

![image](https://github.com/user-attachments/assets/0d598b41-b1f0-48b3-a150-a80a9765b52d)

## PV, PVC

개발자가 Pod를 개발하고

인프라 담당자가 PV를 통해 쿠버네티스 DB등 솔루션 volume 정의하며

PVC는 개발자가 pod에서 필요한 자원을 PV에 요청하는 인터페이스 역할이라고 보면되는데

쿠버네티스를 잘 아는 사람이 적다보니 이 모두를 쿠버네티스 개발자가 하게 된다고 한다.

* local - 노드의 경로를 지정함.
* nodeAffinity - 어느 Node에 pod를 생성할 건지 지정
* volume 솔류션이 많음
* 개발자의 업무분산역할
* 개발자는 PVC를 통해 PV를 이용

![image](https://github.com/user-attachments/assets/be279536-1adc-42f5-be2f-736ef26e951b)

* deployment등을 수정해서 pod의 볼륨으로 hostPath를 지정할 수 있는데. 사용하지 말자. 테스트용으로 정도.
* 원래는 노드에 있는 정보를 Loki 등으로 app이 조회하는 용도
* 

- PV와 PVC를 써서 별도의 스토리지를 만드는 이유 -> pod가 죽어도 파일을 노드에 유지하기 위해

![image](https://github.com/user-attachments/assets/398b6e61-2b61-42df-8708-09383f364e19)

## Deployment

Deployment의 template의 정보를 하나라도 변경하게 되면 업데이트가 진행된다.

만약 version을 2.0으로 변경 시 기존의 ReplicaSet의 1.0의 replicas를 0으로
2.0의 ReplicaSet의 replicas를 deployment에서의 설정 값으로 설정해서 pod를 생성한다.

* strategy
  - Recreate : 기존 pod 종료와 동시에 업데이트한 pod 생성
    -> 서비스 중단됨

  - RollingUpdate : 업데이트한 pod가 생성되면 종료.
    -> 업데이트 중 서비스 중단 없음. 두 버전의 서비스가 동시에 호출될 수 있음. 자원사용량 150%증가

  - Blue/Green
    -> 업데이트 pod가 완전히 모두 생성되면 호출. 자원 200% 증가

  - RollingUpdate option
    * maxUnvailable : 업데이트 동안 최대 몇개의 pod를 유지할 것인지.
    * maxSurge : 새로운 pod를 최대 몇개까지 생성할 것인지.

    (100%, 100%) -> Recreate
    (0%, 100%) -> Blue/Green에 가까운 효과
    (25%, 25%) -> 생성되고 종료되는 중에 서비스 pod개수가 달라질 가능성이 있음.

## Service

![image](https://github.com/user-attachments/assets/895d2288-e843-42fc-a697-e0d73dfb4d04)

type이 NodePort로 설정되면 Node에 port가 생성이 된다.
내부 pod와 포트포워딩이 되며 nodeport로 외부 트래픽과 연결된다.

내부 dns로 service 이름으로 api를 호출하기도하고
namespace 밖의 namespace에서는 namespace명을 붙여주며 호출하기도 한다.

targetport의 http 설정은 pod안의 name과 연결되어 자동으로 container 안의 port와 연결된다.


![image](https://github.com/user-attachments/assets/626dc5ba-ab38-46b7-927c-4ec1ec416da6)

* 서비스 레지스트리 : pod가 종료하고 생성되는 과정에서 ip자동 관리
* 로드밸런싱 : 트래픽을 서비스중인 pod에 적절하게 분담해 부하를 줄여준다.

## HPA

![image](https://github.com/user-attachments/assets/9955f9b2-7e69-4f92-b367-de293a0d6189)

* metric
  - cpu 사용양에 따른 스케일링
    - averaeUtilization: cpu 사용량이 설정값 이상일때 pod 증가

* requests : HPA가 100% 수치로 계산하기 위한 기준 값.
* 스케일링 될 pod량 계산 : 현 pod 수 * (평균cpu/HPA cpu)
* limits : 한 pod에서 최대 cpu사용률 한도. 

* memory는 사용양에 따라 가비지 컬렉션이 일어나면서 사용상황에 따라변경되기 때문에 거의 사용X

![image](https://github.com/user-attachments/assets/003a41f1-e3b7-4427-9932-b92a2285f3cc)

* 부하가 늘면 scaleout을해서 pod를 늘리고 부하가 줄면 scalein해서 pod 감소
* 현실적으로는 부하가 급격하게 늘면 pod가 모두 죽어 서비스가 중단된다.
  -> 시간대 분석해서 pod늘려놓기, 대기열 아키텍처를 구성

* behavior
  scaleUp:
    stabilizationWindowSeconds: 120 -> 2분 동안 metrics의 averageUtilization 설정값 유지 시 scaleout
  scaleDown:
    stabilizationWindowSeconds: 600 -> 10분 동안 부하가 감소해도 pod 개수 유지
  policies:
    - type: Pods 
    value: 1
    periodSeconds: 60 -> 1분에 pod 한개씩 종료


~~~
이 글은 인프런 쿠버네티스 어나더 클래스 복습자료입니다.
~~~
