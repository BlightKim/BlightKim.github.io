---
title: 스프링부트 OAuth2 설정 임시로 생략하기
parent: 백엔드
layout: default
---

# 문제 원인

- h2 데이터베이스 설정 이후 local 에서 스프링부트를 실행하자 다음과 같은 에러 발생

```shell
Description:

Method filterChain in com.simplyrun.config.security.SecurityConfig required a bean of type 'org.springframework.security.oauth2.client.registration.ClientRegistrationRepository' that could not be found.


Action:

Consider defining a bean of type 'org.springframework.security.oauth2.client.registration.ClientRegistrationRepository' in your configuration.
```

원인은 `local` 에서 OAuth2 관련 설정을 생략했기 때문이다.

일단 OAuth2 관련 설정을 임시로 생략하고 개발을 진행하는 걸로 하였다



# 해결 방법

- `Security Config` 파일에서 아래 부분을 주석 처리 한다

```java

        http
            .oauth2Login(oauth2 -> oauth2
                .authorizationEndpoint(endpointConfig -> endpointConfig
                        .baseUri("/oauth2/authorization")
                        .authorizationRequestRepository(oAuthRequestRepository)
                    )
                .userInfoEndpoint(userInfo -> userInfo
                    .userService(oAuthService)
                )
                .successHandler(successHandler)
                .failureHandler(failureHandler)
            );
```


# 결과

```shell
2024-11-01T10:30:07.462+09:00  INFO 85232 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2024-11-01T10:30:07.469+09:00  INFO 85232 --- [           main] com.simplyrun.SimplyrunApiApplication    : Started SimplyrunApiApplication in 3.535 seconds (process running for 3.806)
2024-11-01T10:30:07.512+09:00  INFO 85232 --- [pool-4-thread-1] o.springdoc.api.AbstractOpenApiResource  : Init duration for springdoc-openapi is: 141 ms
2024-11-01T10:30:07.762+09:00  INFO 85232 --- [pool-5-thread-1] o.springdoc.api.AbstractOpenApiResource  : Init duration for springdoc-openapi is: 391 ms
2024-11-01T10:30:07.765+09:00  INFO 85232 --- [pool-3-thread-1] o.springdoc.api.AbstractOpenApiResource  : Init duration for springdoc-openapi is: 394 ms
```

정상적으로 실행되는 것을 확인할 수 있다
