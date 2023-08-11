---
title: Mockito를 활용한 단위테스트
layout: default
parent: 스프링부트 + 시큐리티 + 리액트를 이용한 블로그 만들기
grand_parent: 백엔드
---

# Mockito란 ?

Mockito란 Mock을 지원하는 프레임워크이다. 구현체가 없거나 있더라도 특정 단위만 테스트 하고 싶을 때 사용한다.

> Mockito의 특징

- Mockito는 간단하고 읽기 쉬운 API를 제공하고 Mock 객체를 쉽게 생성, 관리할 수 있다.
- when() 등의 메서드를 사용하여 mock 객체에 대한 특정 호출에 대한 반환 값을 제어할 수 있다. 이를 소프트웨어 용어로 stubbing 이라고 한다.

----  
# 의존성 주입

스프링부트를 사용하고 있다면 spring-boot-starter-test가 mockito를 자동으로 주입시켜준다.  
  
스프링부트가 아니라면 다음과 같이 라이브러리를 추가한다.

```gradle
    testImplementation "org.mockito:mockito-core:3.+"
```
  
----
  
# Mockito 사용법

1. Mock 객체를 생성한다.
  
```java
@ExtendWith(MockitoExtension.class)
class AuthServiceTest {
    @InjectMocks
    AuthService authService;

    @Mock
    MemberRepository memberRepository;

    @Mock
    PasswordEncoder passwordEncoder;
}
```

{: .important }
> A paragraph
>
> Another paragraph
>
> The last paragraph