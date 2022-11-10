## BOJ 2293. 동전 1

### 문제 링크

[[BOJ 2293\] 동전 1](https://www.acmicpc.net/problem/2293)

### 분류

다이나믹 프로그래밍

### 코드

```python
n, k = map(int, input().split())
coins = [int(input()) for _ in range(n)]

dp = [0] * (k + 1)      # dp[j]: j원이 되는 경우의 수
dp[0] = 1

for i in coins:                 # i: 동전 값
    for j in range(i, k + 1):   # j: dp 인덱스
        dp[j] += dp[j - i]

print(dp[k])
```

