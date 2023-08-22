---
layout: default
title: 4. 숫자만 추출
parent: 코딩테스트 연습
---
# 문제
문자와 숫자가 섞여있는 문자열이 주어지면 그 중 숫자만 추출하여 그 순서대로 자연수를 만듭니다.

만약 “tge0a1h205er”에서 숫자만 추출하면 0, 1, 2, 0, 5이고 이것을 자연수를 만들면 1205이 됩니다.



# 나의 풀이
```java
import java.util.*;
import java.util.Scanner;

class Main {
    public int solution(String str) {
        str = str.replaceAll("[^0-9]", "");
        if(str.charAt(0) == '0') {
            str = str.substring(0, str.length());
        }
        int answer = Integer.parseInt(str);
        return answer;
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.nextLine();
        System.out.println(T.solution(str));
    }
}
```


# 풀이 1
> 참고사항
>
> '0' <= x <= '9' : 48 <= x && x <=57

```java
import java.util.*;
import java.util.Scanner;

class Main {
    public int solution(String str) {
        int answer = 0;
        for(char x : str.toCharArray()) {
            if (48 <= x && x <= 57) {
                answer = answer * 10 + (x - 48);
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.nextLine();
        System.out.println(T.solution(str));
    }
}
```

# 풀이 2
```java
import java.util.*;
import java.util.Scanner;

class Main {
    public int solution(String str) {
        String answer = "";
        for (char x : str.toCharArray()) {
            if (Character.isDigit(x)) {
                answer += x;
            }
        }
        return Integer.parseInt(answer);
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.nextLine();
        System.out.println(T.solution(str));
    }
}
```