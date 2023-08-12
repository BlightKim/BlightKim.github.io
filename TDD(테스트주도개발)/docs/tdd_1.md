---
title: 1.계산기 만들기
layout: default
nav_order: 4.5
parent: TDD(테스트주도개발)
---

# 1. 계산기 코드 작성

```java
public class CalculationRequestReader {
  public String[] read() {
    Scanner scanner = new Scanner(System.in);
    System.out.println("Enter two numbers and an operator (e.g 1 + 2): ");
    String result = scanner.nextLine();
    return result.split(" ");
  }
}
```

{: .highlight }
`<CalculationRequestReader>` -> 숫자와 연산자를 화면으로 입력받는 클래스

```java
public class InvalidOperatorException extends RuntimeException {

  public InvalidOperatorException() {
    super("Invalid operator, you need to choose one of (+,-,*,/)");
  }
}
```

{: .highlight }
`<InvalidOperatorException>` -> 잘못된 연산자를 입력했을 경우 Exception을 발생시킨다.

```java
public class Calculator {
  public long calculate(long num1, String operator, long num2) {
    return switch (operator) {
      case "+" -> num1 + num2;
      case "-" -> num1 - num2;
      case "*" -> num1 * num2;
      case "/" -> num1 / num2;
      default -> throw new InvalidOperatorException();
    };
  }
}
```

{: .highlight }
`<Calculator>` -> 입력받은 연산자에 해당하는 연산을 수행하는 클래스

# 2. 테스트코드 작성

```java
  @Test
  public void 덧셈_연산을_할_수_있다() {
    // given
    long num1 = 2;
    String operator = "+";
    long num2 = 3;
    Calculator calculator = new Calculator();

    // when
    long result = calculator.calculate(num1, operator, num2);

    // then
    assertEquals(5, result);
    assertThat(result).isEqualTo(5);
  }

@Test
  public void 잘못된_연산자가_요청으로_들어올_경우() {
    // given
    long num1 = 6;
    String operator = "x";
    long num2 = 3;
    Calculator calculator = new Calculator();

    // then
    assertThrows(InvalidOperatorException.class, () -> calculator.calculate(num1, operator, num2));
    assertThatThrownBy(() -> calculator.calculate(num1, operator, num2)).isInstanceOf(
        InvalidOperatorException.class);
  }
```

> 위와 같이 덧셈과 에러 발생 테스트를 작성한다

Tip
{: .label}

```java
  @Test
  public void System_in으로_데이터를_읽어들일_수_있다() {
    // given
    System.setIn(new ByteArrayInputStream("2+3".getBytes()));
  }
```

> System.setIn(new ByteArrayInputStream("2+3".getBytes()));  
> Scanner에서 값을 입력받는 상황일 때 위와 같이 구현 가능하다.
