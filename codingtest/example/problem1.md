---
layout: default
title: 1. 정수 부분 구하기
parent: 코딩테스트 연습
---
# 문제

실수 `flo`가 매개 변수로 주어질 때, `flo`의 정수 부분을 return하도록 solution 함수를 완성해주세요.

```java
class Solution {
    public int solution(double flo) {
        int answer = 0;
        return answer;
    }
}
```

# 풀이

실수에 100을 곱하고 다시 100으로 나눈다. double에서 int로 바꾸기 위해서 형변환을 해야한다.

# 나의 풀이

```java
class Solution {
    public int solution(double flo) {
        int answer = 0;
        answer = (int)(flo * 100) / 100;
        return answer;
    }
}
```
