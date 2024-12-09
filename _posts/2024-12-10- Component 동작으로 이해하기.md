---
layout: post
title: Component 동작으로 이해하기
categories: object kubelet kube-controller-manager log
description: Component 동작으로 이해하기
keywords: object kubelet kube-controller-manager log
---

Component 동작으로 이해하기

우선 설치를 다시 해야한다.. 까먹었다..

~~~
windows > cmd 실행, 디렉토리생성
C:\Users\사용자> mkdir k8s && cd k8s

curl 명령어로 vagrant 스크립트 다운로드
curl -O https://raw.githubusercontent.com/k8s-1pro/install/main/ground/k8s-1.27/vagrant-2.3.4/Vagrantfile

Rocky Linux 세팅
curl -O https://raw.githubusercontent.com/k8s-1pro/install/main/ground/k8s-1.27/vagrant-2.3.4/rockylinux-repo.json
vagrant box add rockylinux-repo.json

Vagrant disk 설정 플러그인 설치
vagrant plugin install vagrant-vbguest vagrant-disksize

Vagrant 실행 (VM생성)
vagrant up
~~~

![image](https://github.com/user-attachments/assets/fbd27895-a7c2-482a-8620-93e0a4081040)

굿





## Object 생성과정

![image](https://github.com/user-attachments/assets/c6742f51-aa97-43c6-b4bc-de85d7fc2108)

![image](https://github.com/user-attachments/assets/225c0e49-499d-4113-bfd0-961970b6c87e)
kubectl -> deployment 생성해! -> kube-apiserver -> etcd : 데이터 저장
kube-controller-manager : dataset 모니터링 -> deployment 조회 -> replicaset 생성해!


![image](https://github.com/user-attachments/assets/5fd3b522-1a46-4018-9397-b2bd809158be)
kube-scheduler : kube-apiserver 모니터링 -> Pod 띄울 노드 스케쥴링

![image](https://github.com/user-attachments/assets/3ae4838e-1356-4c50-8a9a-9017ed9d8525)
kubelet : kube-apiserver 모니터링 -> 컨테이너 생성해줘 -> containerd : 컨테이너 생성
probe 설정 -> 컨테이너 헬스체크 api 주기적 전달

![image](https://github.com/user-attachments/assets/d40b778a-d3e0-4ff1-babc-b4c2d661c843)
서비스 type: nodeport
kubelet -> 서비스 요청 -> kube-proxy : iptables port 맵핑
calico : 컨테이너로 트래픽 전달 

secret
- 데이터가 많이 쌓여 용량을 많이 차지 할 수 있음
- 바로 적용이아니라 kubelet에서 모니터링 후 변경

자원체크
- kubelet이 10초에 한번씩 조회
- metrics-server 설치를 해야 60초에 한번씩 모니터링
- kube-controller-manager가 모니터링 한 후 HPA의 임계값과 메트릭을 15초 단위로 비교-> 스케일링
- 최악의 경우 85초 후 스케일링

▶ Cluster 주요 컴포넌트 로그 확인

// 주요 컴포넌트 로그 보기 (kube-system)
kubectl get pods -n kube-system
kubectl logs -n kube-system etcd-k8s-master
kubectl logs -n kube-system kube-scheduler-k8s-master
kubectl logs -n kube-system kube-apiserver-k8s-master
...

// 쿠버네티스 인증서 위치
cd /etc/kubernetes
ls /root/.kube/config
~~~
- 인증서 복사위치를 읽는다

// 상태 확인 -> 상세 로그 확인 -> 10분 구글링 -> VM 재기동 -> Cluster 재설치 ->  답을 찾을 때 까지 구글링
- 왠만하면 재설치 -> 익숙해지면 구글링 순

// Log 확인
* 3-1 kubectl logs -n anotherclass-123 <pod-name> --tail 10    // 10줄 만 조회하기
* 3-2 kubectl logs -n anotherclass-123 <pod-name> -f           // 실시간으로 조회 걸어 놓기
* 3-3 kubectl logs -n anotherclass-123 <pod-name> --since=1m   // 1분 이내에 생성된 로그만 보기

이 글은 인프런 쿠버네티스 어나더 클래스 복습자료입니다.
~~~
