## BOJ 2170. 선 긋기

### 문제 링크

[[BOJ 2170] 선 긋기](https://www.acmicpc.net/problem/2170)

### 분류

정렬, 스위핑

### 코드

``` python
import sys
input = sys.stdin.readline


N = int(input())

lines = []
ans = 0

for _ in range(N):
    tmp = list(map(int, input().split()))
    lines.append(tmp)

lines.sort()

pre_start = lines[0][0]
pre_end = lines[0][1]

for start, end in lines:
    if start <= pre_end:
        pre_end = max(pre_end, end)
        
    else:
        ans += pre_end - pre_start
        pre_start = start
        pre_end = end

ans += pre_end - pre_start

print(ans)
```