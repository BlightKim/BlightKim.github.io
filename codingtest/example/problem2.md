---
layout: default
title: 2. 회문문자열
parent: 코딩테스트 연습
---
# 문제
앞에서 읽을 때나 뒤에서 읽을 때나 같은 문자열을 회문 문자열이라고 합니다.

문자열이 입력되면 해당 문자열이 회문 문자열이면 "YES", 회문 문자열이 아니면 “NO"를 출력하는 프로그램을 작성하세요.

단 회문을 검사할 때 대소문자를 구분하지 않습니다.


# 나의 풀이
```java
import java.util.*;
import java.util.Scanner;

class Main {
    public String solution(String str) {
        int maxCount = (str.length()) / 2;
        int count = 0;
        int fIdx = 0;
        int bIdx = str.length() - 1;

        while (count < maxCount) {
            if ((String.valueOf(str.charAt(fIdx))).equalsIgnoreCase(String.valueOf(str.charAt(bIdx)))) {
                fIdx++;
                bIdx--;
                count++;
            } else {
                return "NO";
            }
        }

        return "YES";
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.next();
        System.out.println(T.solution(str));


    }
}
```

# 풀이 1
```java
import java.util.*;
import java.util.Scanner;

class Main {
    public String solution(String str) {
        String answer = "YES";
        str = str.toUpperCase();
        int len = str.length();

        for(int i = 0; i < len/2; i++) {
            if(str.charAt(i) != str.charAt(len - i - 1)) {
                return "NO";
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.next();
        System.out.println(T.solution(str));


    }
}
```

# 풀이 2
```java
import java.util.*;
import java.util.Scanner;

class Main {
    public String solution(String str) {
        String answer = "YES";
        String tmp = new StringBuilder(str).reverse().toString();
        if (str.equalsIgnoreCase(tmp))
            return "YES";
        else
            return "NO";
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        String str = kb.next();
        System.out.println(T.solution(str));


    }
}
``` 