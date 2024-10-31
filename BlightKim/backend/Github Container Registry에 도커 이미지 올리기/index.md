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

