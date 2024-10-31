---
title: 오라클 클라우드에 Docker 설치하기
parent: 백엔드
layout: default
---

# 설치 과정

1. 우분투 시스템 패키지 업데이트

```shell
sudo apt update
```

2. Docker 공식 GPG 키 추가

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

3. Docker 공식 apt 저장소 추가

> 나는 오라클 클라우드 arm 아키텍처 인스턴스를 사용한다
> 
> 때문에 해당 아키텍처에 맞는 저장소를 설치해줘야한다


```shell
sudo add-apt-repository "deb [arch=arm64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```


4. Docker 설치

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

5. Docker 설치 확인

```shell
sudo systemctl status docker

● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset>
     Active: active (running) since Thu 2024-10-31 01:31:14 UTC; 8s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 14400 (dockerd)
```

정상적으로 실행되는 것을 확인할 수 있다

