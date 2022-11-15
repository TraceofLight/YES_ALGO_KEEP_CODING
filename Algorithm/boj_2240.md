## BOJ 2240. 자두나무

### 문제 링크

[[BOJ 2240] 자두나무](https://www.acmicpc.net/problem/2240)

### 분류

다이나믹 프로그래밍

### 코드

``` python
T, W = list(map(int, input().split()))
jadu_list = [0]
ans_list = [[0 for _ in range(T+1)] for _ in range(W+1)]

for _ in range(T):
    jadu_list.append(int(input()))

# 0번 움직임
for idx in range(1, T+1):
    if jadu_list[idx] == 1:
        ans_list[0][idx] = ans_list[0][idx - 1] + 1
    else:
        ans_list[0][idx] = ans_list[0][idx - 1]

for susuk in range(1, W+1):
    for idx in range(1, T + 1):
        if jadu_list[idx] == susuk % 2 + 1:
            ans_list[susuk][idx] = max(ans_list[susuk - 1][idx - 1], ans_list[susuk][idx - 1]) + 1
        else:
            ans_list[susuk][idx] = max(ans_list[susuk - 1][idx - 1], ans_list[susuk][idx - 1])

ans = 0
for idx in range(W+1):
    if ans < ans_list[idx][T]:
        ans = ans_list[idx][T]

print(ans)
```