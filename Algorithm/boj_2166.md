## BOJ 2166. 다각형의 면적

### 문제 링크

[[BOJ 2166\] 다각형의 면적](https://www.acmicpc.net/problem/2166)

### 분류

기하학, 다각형의 넓이

### 코드

```python
N = int(input())
P = [tuple(map(int, input().split())) for _ in range(N)]

p1 = P[0]
P.append(p1)

area = 0
for i in range(N):
    area += P[i][0] * P[i+1][1] - P[i][1] * P[i+1][0]
area /= 2

print(round(abs(area), 1))
```

