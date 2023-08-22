---
title: Mockito를 활용한 단위테스트
layout: default
parent: 백엔드
grand_parent: 포트폴리오
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

1. @Mock {: .label } : Mockito.mock(MemberService.class) 을 간단하게 애노테이션으로 만들 수 있다.

1. @ExtendWith {: .label} (MockitoExtension.class) : @Mock 애노테이션을 처리해주는 확장 모델이다.
 

----

# Stubbing 조작하는 법
- 리턴 타입이 객체 이면, null 을 리턴한다.
- 리턴 타입이 Optional 이면, Optional.empty() 를 리턴한다.
- 리턴 타입이 primitive 타입이면, 기본 Primitive 값 을 리턴한다. (int 는 0, boolean은 false)
- 리턴 타입이 콜렉션 이면, 비어있는 콜렉션 을 리턴한다.
- 리턴 타입이 void 면, 예외를 던지지 않고 아무런 일도 발생하지 않는다.

### 1. return 있는 메소드 테스