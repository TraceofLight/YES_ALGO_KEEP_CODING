## BOJ 1934. 최소공배수

### 문제 링크

[[BOJ 1934\] 최소공배수](https://www.acmicpc.net/problem/1934)

### 분류

수학, 정수론, 유클리드 호제법

### 최대공약수와 최소공배수

최대공약수(GCD, Greatest Common Devider)를 먼저 구한다면 최소공배수(LCM, Least Common Multiple)는 비교적 쉽게 구할 수 있다.

일반적으로 손으로 풀 때는 `소인수분해`를 하여 최대공약수를 구하지만, 이를 코드로 구현하려고 하면 숫자가 커질수록 실행 시간이 매우 길어지는 문제가 있다.

이에 따라 시간 단축을 위한 방법으로, `유클리드 호제법`을 이용할 수 있다.

### 유클리드 호제법

유클리드 호제법은 두 수의 최대공약수를 구하는 알고리즘이다.

두 수를 서로 반복하여 나눈 나머지를 이용하여 최대공약수를 구할 수 있다!

예를 들어, 120과 32의 최대공약수를 유클리드 호제법으로 구해보자.

```
120 % 32 = 24      # 나머지 24로 다시 32을 나눔
32 % 24 = 8        # 나머지 8로 다시 24를 나눔
24 % 8 = 0         # 8이 최대공약수 !
```

### 유클리드 호제법의 원리?

아래 그림을 참고한다.

두 숫자 1112와 695의 최대공약수를 n이라 하면, 두 숫자는 모두 n의 배수이다.

1112를 695로 나눈 나머지로 다시 695를 나누고...를 반복하면 마지막에는 n과 0이 남게 되어 최대공약수 n을 구할 수 있다.

[![img](./images/boj_1934_image.gif)](https://github.com/kimsj-git/home/blob/master/1110_study/220813/assets/boj_1934_image.gif)

### 파이썬으로 GCD, LCM 구현

```
def gcd(x, y):
    while y:
        x, y = y, x % y
    return x

def lcm(x, y):
    return int(x*y / gcd(x, y))
```

### 코드

```python
import sys
input = sys.stdin.readline

def gcd(x, y):
    while y:
        x, y = y, x % y
    return x

def lcm(x, y):
    return int(x*y / gcd(x, y))

T = int(input())

for tc in range(T):
    A, B = map(int, input().split())
    print(lcm(A, B))
```

