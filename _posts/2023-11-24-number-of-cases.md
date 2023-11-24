---
title: 경우의 수를 구하는 법
date: 2023-11-22
categories: [blog, math]
tags: [chatgpt]
---
## 경우의 수를 위한 선수 지식

1. 팩토리얼
2. 조합 (Combinations)
3. 순열 (Permutations)

## 팩토리얼

### 팩토리얼이란?
팩토리얼은 어떤 숫자에 대해, 그 숫자부터 1까지 모든 숫자를 곱하는 것이다.

이를 수학적으로 표현하면,

| 팩토리얼            | 수식                                |
| ------------------- | ----------------------------------- |
| n! (n 팩토리얼)     | n × (n-1) × (n-2) × ... × 3 × 2 × 1 |
| 4 팩토리얼 (4!)     | 4 × 3 × 2 × 1 = 24                  |
| 3 팩토리얼 (3!)     | 3 × 2 × 1 = 6                       |
| 2 팩토리얼 (2!)     | 2 × 1 = 2                           |
| 1 팩토리얼 (1!)     | 1 = 1                               |
| **0 팩토리얼 (0!)** | 1                                   |

팩토리얼의 특별한 규칙:
**0 팩토리얼 (0!)**은 항상 1이다. 이것은 수학적인 정의에 따른 것.


### 자바에서 팩토리얼 구현하기:



<details markdown="block"><summary> for 문 </summary>

```java
public class FactorialExample {
    public static void main(String[] args) {
        int number = 4;
        int factorial = 1;

        for (int i = 1; i <= number; i++) {
            factorial = factorial * i;
        }

        System.out.println("4의 팩토리얼은: " + factorial);
    }
}
```
</details>

<details markdown="block"><summary> 재귀 함수 </summary>

```java
public class Factorial {
    public static long factorial(int n) {
        if (n <= 1) { // 기저 조건
            return 1;
        } else {
            return n * factorial(n - 1); // 재귀 호출
        }
    }

    public static void main(String[] args) {
        int number = 5;
        System.out.println("5의 팩토리얼은: " + factorial(number));
    }
}
```
</details>

<details markdown="block"><summary> 스트림 API 사용 (Java 8 이상) </summary>

```java
import java.util.stream.LongStream;

public class Factorial {
    public static long factorial(int n) {
        return LongStream.rangeClosed(1, n)
                         .reduce(1, (long a, long b) -> a * b);
    }

    public static void main(String[] args) {
        int number = 5;
        System.out.println("5의 팩토리얼은: " + factorial(number));
    }
}
```
</details>

## 조합 

### 조합이란 ?
조합은 서로 다른 n개의 원소 중에서 순서에 상관없이 r개를 선택하는 **경우의 수**이다. 

> C(n,r)= 
r!×(n−r)!
n!
​


```java
public class Combination {
    // 팩토리얼 계산 메소드
    public static long factorial(int number) {
        long result = 1;
        for (int factor = 2; factor <= number; factor++) {
            result *= factor;
        }
        return result;
    }

    // 조합 계산 메소드
    public static long combination(int n, int r) {
        return factorial(n) / (factorial(r) * factorial(n - r));
    }

    public static void main(String[] args) {
        int n = 5;
        int r = 3;
        System.out.println("Combination of 5 choose 3 is: " + combination(n, r));
    }
}
```


## 순열 

### 순열이란 ?
순열은 서로 다른 n개의 원소 중에서 순서에 따라 r개를 선택하는 **경우의 수**이다.

> P(n,r)= 
(n−r)!
n!
​


```java
public class Permutation {
    // 팩토리얼 계산 메소드
    public static long factorial(int number) {
        long result = 1;
        for (int factor = 2; factor <= number; factor++) {
            result *= factor;
        }
        return result;
    }

    // 순열 계산 메소드
    public static long permutation(int n, int r) {
        return factorial(n) / factorial(n - r);
    }

    public static void main(String[] args) {
        int n = 5;
        int r = 3;
        System.out.println("Permutation of 5 choose 3 is: " + permutation(n, r));
    }
}


```