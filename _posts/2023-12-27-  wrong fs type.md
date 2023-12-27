---
layout: post
title: In linux mounting xfs disk at /etc/fstab to auto mount
categories: linux mount /etc/fstab xfs nfs ext4
description: linux mount /etc/fstab xfs nfs ext4
keywords: linux mount /etc/fstab xfs nfs ext4
---

# auto mount
재부팅 시 마운트 해제 되어있는 경우를 발견했다.
/etc/fstab 을 수정하여 마운트 하려는데 
오류가 발생했다.

## Error wrong fs type 
~~~
mount: /data1: wrong fs type, bad option, bad superblock on /dev/sdb1, missing codepage or helper program, or other error.
~~~

## ext4 변환
한국어로 써있는 블로그에는 ext4로 변환하면 된다는데 데이터가 모두 없어진다고 한다.
-- 패스

~~~
#ex
sudo mkfs.ext4 /dev/sda 
~~~

## install nfs-common
askubuntu에 NFS타입의 directories는 nfs-common을 설치하면 된다고한다.
~~~
sudo apt-get install nfs-common
~~~

> 위 방법을 사용
~~~
sudo fdisk -l 
~~~
을 사용해서 각 디스크 정보를 알아낸 후에

~~~
sudo blkid
~~~
로 각 disk의 UUID를 매칭하여

/etc/fstab 에 다음과 같이 수정했다

~~~
UUID= 디스크UUID /data(mount할 폴더) xfs(디스크타입) default 0 0
~~~

그리고 다음과 같이 마운트 적용을 했다.
~~~
sudo mount -a
~~~