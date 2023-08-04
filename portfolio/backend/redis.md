---
title: Refresh 토큰과 레디스 템플릿 사용
layout: default
parent: 백엔드
grand_parent: 포트폴리오
---

# Redis를 사용하게 된 계기

Refresh 토큰을 관리하기 위한 데이터베이스로 MongoDB, Redis 중에 고민을 했고 Redis를 사용하기로 결정했다.

# Redis는 무엇일까 ?

- Redis는 NoSQL(기존 RDBMS 방식을 탈피한 데이터베이스를 의미)이다.
- 메모리 기반 데이터베이스

# NoSQL의 종류

- 서로 연관된 그래프 형식의 데이터를 저장할 수 있는 Graph Store
- Row가 아닌 Column 위주로 데이터를 저장하는 Column Store
- 비정형 대량 데이터를 저장하기 위한 Document Store
- 메모리 기반으로 빠르게 데이터를 읽어올 수 있는 Key-Value Store

> > **_REDIS_** 는 Key-Value Store에 속한다.

그렇다면 자바 HashMap과 별 차이가 없는거 아닌가 ? 라는 생각이 들었다. 메모리 기반, key-value 구조로 보면 HashMap과 일치하기 때문이다.

> > Redis 설정은 다음과 같다

```java
package com.sebin.board.config;

import lombok.Getter;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.lettuce.LettuceConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.repository.configuration.EnableRedisRepositories;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Getter
@Configuration
@RequiredArgsConstructor
public class RedisConfig {
    @Value("${spring.data.redis.host}")
    private String host;

    @Value("${spring.data.redis.port}")
    private int port;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());

        // 일반적인 key:value의 경우 시리얼라이저
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());

        // Hash를 사용할 경우 시리얼라이저
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new StringRedisSerializer());

        // 모든 경우
        redisTemplate.setDefaultSerializer(new StringRedisSerializer());

        return redisTemplate;
    }
}
```

레디스 템플릿은 사용하는 자료구조마다 제공하는 메서드가 다르기 때문에 객체를 만들어서 레디스의 자료구조 타입에 맞는 메소드를 사용하면 된다.

| 메서드명    | 레디스 타입 |
| :---------- | :---------- |
| opsForValue | String      |
| opsForList  | Set         |
| opsForZSet  | Sorted Set  |
| opsForHash  | Hash        |

데이터를 저장할 때 만료 시간 지정할 시에는 해당 시간의 단위까지 지정해주면 된다. 위의 코드에서는 밀리 초(TimeUnit.MILLISECONDS)로 적용되어 있다.
