---
title: 테스트컨테이너
layout: default
parent: 백엔드
grand_parent: 포트폴리오
---

# 테스트 컨테이너 구축 배경

### TestContainers 특징

- Java로 Container를 동작시킬 수 있다.
- Dockerfile, docker-compose, docker hub로 Container를 동작시킬 수 있다.
- 테스트 실행 전/후로 Container를 start, stop 할 수 있다.
- Parallel Test를 지원한다.
- 다양한 module을 제공하고 있다.

> Testcontainers 관련 라이브러리 설치

```gradle
    testImplementation "org.junit.jupiter:junit-jupiter:5.8.1"
    testImplementation "org.testcontainers:testcontainers:1.18.3"
    testImplementation "org.testcontainers:junit-jupiter:1.18.3"
```
