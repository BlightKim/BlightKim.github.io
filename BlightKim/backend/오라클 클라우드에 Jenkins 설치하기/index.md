---
title: 오라클 클라우드에 Jenkins 설치하기
parent: 백엔드
layout: default
---
# 들어가며

현재 오라클은 다음과 같은 무료 인스턴스를 제공하고 있다

- ARM : 최대 4코어 / 램24GB

0r

- AMD : 최대 1코어 / 램 2GB

분할 사용이 가능하기 때문에 나는 `2코어 / 램 12GB` 인스턴스를 2개 생성했다

하나는 `웹 서버` 나머지 하나는 `Jenkins 등` 다른 소프트웨어를 설치하는 용도이다

Jenkins 설치 과정이 복잡해서 기록으로 남기려고 한다

# 설치 과정

1. 우분투에 자바를 설치한다

```shell
apt-get install temurin-21-jdk
```

실행 후 다음과 같은 에러 메시지가 출력되고 설치가 진행되지 않았다

```shell
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package temurin-21-jdk
```

이유는 리포지토리가 설치되지 않았기 때문이다

리포지토리를 등록해준다

```shell
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo apt-key add -
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update
sudo apt-get install temurin-21-jdk
```

자바가 제대로 설치 되었는지 확인한다

```shell
java -version
openjdk version "21.0.4" 2024-07-16 LTS
OpenJDK Runtime Environment Temurin-21.0.4+7 (build 21.0.4+7-LTS)
OpenJDK 64-Bit Server VM Temurin-21.0.4+7 (build 21.0.4+7-LTS, mixed mode, sharing)
```

정상적으로 자바가 설치되었다

2. 
