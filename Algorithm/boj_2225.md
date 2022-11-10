## BOJ 2225. 합분해

### 문제 링크

[[BOJ 2225\] 합분해](https://www.acmicpc.net/problem/2225)

### 분류

수학, 다이나믹 프로그래

### 코드

```python
'''
중복 조합의 개수를 이용한 풀이.
N개의 1을 K개의 그룹으로 나누는 경우의 수를 센다.
 1, 1, ... 1, 1
^  ^  ^   ^  ^  ^  <= 이 위치들 중 K-1개를 꺼냄
0도 포함할 수 있으므로, 중복되는 K-1개를 선택한다. 
'''
from math import factorial


def nHk(n, k):
    return factorial(n + k - 1) // factorial(n - 1) // factorial(k)


N, K = map(int, input().split())
print(nHk(N + 1, K - 1) % 1000000000)
```

