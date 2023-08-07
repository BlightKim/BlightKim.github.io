---
title: Learning 코틀린 1
layout: default
parent: 코틀린
---

# 코틀린이란 ?

코틀린은 개발자 도구 전문 기업인 JetBrains가 만든 언어다.

코틀린은 다음과 같은 여러 프로그래밍 패러다임을 지원한다.

- 명령형 프로그래밍
- 함수형 프로그래밍
- 객체 지향 프로그래밍

# 코틀린의 특징

- 코틀린은 자바와 완전한 상호성을 지원한다. 코틀린은 노력 없이 기존 자바 프로젝트에 코틀린을 통합 할 수 있다.
- 코틀린은 NullPointer Exception을 발생시킬 가능성이 있는 연산을 컴파일 시점에 잡아 낸다. NullPointer Exception은 수 많은 개발자들을 괴롭히는 문제였다.

# HelloWorld 출력

```kotlin
fun main() {
println("Hello, World");
}
```

코틀린으로 Hello, World를 출력하는 코드이다.

```java
public class Hello {

  public static void main(String[] args) {
    System.out.println("Hello World");
  }
}

```

다음은 자바 코드이다. 상당히 축약되고 간소화 된 것을 알 수 있다.

# 변수 방식의 차이

```kotlin
fun main() {
    var sum = 1;
    sum = sum + 2;
    sum += 3;
    println(sum);

}
```

변수 사용 방식을 보면 특이한 점이 있다. 자바는 변수를 생성할 때 자료형을 명시해줘야 했다. 문자열이면 Text, 정수형이면 int, long 등등, 코틀린은 var 변수 하나로 다양한 종류의 자료를 저장할 수 있다.
