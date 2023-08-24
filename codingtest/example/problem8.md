---
layout: default
title: 8. 큰 수 출력하기
parent: 코딩테스트 연습
---
# 문제
N개의 정수를 입력받아, 자신의 바로 앞 수보다 큰 수만 출력하는 프로그램을 작성하세요.

(첫 번째 수는 무조건 출력한다)

# 나의 풀이
# 풀이
 ```java
import java.util.ArrayList;
import java.util.Scanner;

class Main {
    public ArrayList<Integer> solution(int n, int[] arr) {
        ArrayList<Integer> answer = new ArrayList<>();
        answer.add(arr[0]);
        for(int i = 1; i < n; i++) {
            if(arr[i] > arr[i-1]) {
                answer.add(arr[i]);
            }
        }


        return answer;
    }

    public static void main(String[] args) {
        Main T = new Main();
        Scanner kb = new Scanner(System.in);
        int n = kb.nextInt();
        int[] arr = new int[n];

        for(int i = 0; i < n; i++) {
            arr[i] = kb.nextInt();
        }

        for(int x : T.solution(n, arr)) {
            System.out.print(x + " ");
        }
    }
}
 ```