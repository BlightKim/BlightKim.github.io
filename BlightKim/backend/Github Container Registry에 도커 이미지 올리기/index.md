---
title: Github Container Registry에 도커 이미지 올리기
parent: 백엔드
layout: default
---
# 들어가며

Docker Hub에서 무료로 제공하는 Private Repository는 1개다 

Public Repository에 저장하자니 보안이 걱정되었다

Docker Hub는 백엔드 이미지 저장소로 사용하기로 결정하고 프론트 이미지 저장소를 찾아보기로 했다

# GitHub Container Registry

- `GitHub Container Registry`는 Docker Container 이미지를 안전하게 저장하고 관리 할 수 있다

1. `GitHub Container Registry`에 로그인한다

```shell
docker login ghcr.io -u [계정이름]

Password: [github에서 발급받은 토큰]
```

2. 태그 명을 `Github Container Registry` 형식에 맞춰서 지정하여 빌드한다

```shell
#  Buildx 활성화
sudo docker buildx create --use

# QEMU는 Docker가 여러 아키텍처에서 이미지를 빌드하고 실행할 수 있도록 지원하는 에뮬레이터
sudo docker run --rm --privileged multiarch/qemu-user-static --reset -p yes

# 다중 아키텍처 이미지 빌드

docker buildx build --platform linux/amd64,linux/arm64 -t [username]/[image_name]:[tag] . --push
```
