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

> Test 폴더에 다음과 같이 Configuration 파일을 생성한다.
```java
package com.sebin.board.config.code;

import org.junit.jupiter.api.extension.BeforeAllCallback;
import org.junit.jupiter.api.extension.ExtensionContext;
import org.testcontainers.containers.GenericContainer;
import org.testcontainers.utility.DockerImageName;

public class TestContainerConfig implements BeforeAllCallback {

  private static final int REDIS_PORT = 6379;
  private GenericContainer redis;

  @Override
  public void beforeAll(ExtensionContext context) {
    redis = new GenericContainer(DockerImageName.parse("redis:6-alpine"))
        .withExposedPorts(REDIS_PORT);
    redis.start();
    System.setProperty("spring.data.redis.host", redis.getHost());
    System.setProperty("spring.data.redis.port", String.valueOf(redis.getMappedPort(REDIS_PORT
    )));
  }
}
```

> 테스트가 실행되기 전 @ExtendWith(TestContainerConfig.class) TestConfiguration 파일을 실행하여 Redis를 시작하도록 하고, 테스트가 끝나면 Docker를 종료시킨다.
```java
package com.sebin.board.config;



import com.sebin.board.config.code.TestContainerConfig;
import com.sebin.board.service.RedisService;
import com.sebin.board.service.RedisServiceImpl;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.context.SpringBootTest.WebEnvironment;

import org.springframework.context.annotation.Import;
import org.springframework.data.redis.core.RedisTemplate;


@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@Import(RedisConfig.class)
@ExtendWith(TestContainerConfig.class)
class RedisTest {

  @Autowired
  private RedisTemplate<String, Object> redisTemplate;

  private RedisService redisService;

  @BeforeEach
  public void beforeEach() {
    redisService = new RedisServiceImpl(redisTemplate);
  }

  @Test
  public void test() {
    redisService.setData("세빈", "수진", 1000 * 60 * 600L);
    String value = redisService.getDate("세빈");
    Assertions.assertThat(value).isEqualTo("수진");
  }
}
```
