---
layout: default
title: 5. 문자거리
parent: 코딩테스트 연습
---
# 문제
한 개의 문자열 s와 문자 t가 주어지면 문자열 s의 각 문자가 문자 t와 떨어진 최소거리를 출력하는 프로그램을 작성하세요.

{: .highlight }
입력 값 : teachermode e

{: .highlight}
출력 값 : 1 0 1 2 1 0 1 2 2 1 0


# 나의 풀이(시간 초과) --> 복습 필요


# 풀이 1
```java
import java.util.Scanner;

class Main {
    public int[] solution(String str, char c) {
        int[] answer = new int[str.length()];
        int p = 1000;
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == c) {
                p = 0;
                answer[i] = 0;
            } else {
                p++;
                answer[i] = p;
            }
        }

        p = 1000;

        for (int i = str.length() - 1; i >= 0; i--) {
            if (str.charAt(i) == c)
                p = 0;
            else {
                p++;
                answer[i] = Math.min(answer[i], p);
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.next();
        char c = kb.next().charAt(0);
        for (int x : T.solution(str, c)) {
            System.out.print(x + " ");
        }
    }
}
```