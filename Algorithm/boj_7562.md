## BOJ 7562. 나이트의 이동

### 문제 링크

[[BOJ 7562\] 나이트의 이동](https://www.acmicpc.net/problem/7562)

### 분류

그래프 이론, 그래프 탐색, 너비 우선 탐색

### 코드

```python
# BFS 함수: 체스판 한 변 길이(n), 시작점(start), 종점(end) 받아서
# 최소 이동 횟수 반환
def BFS(n, start, end):
    count = [[None] * n for _ in range(n)]
    queue = [start]
    count[start[0]][start[1]] = 0

    while queue:
        v = queue.pop(0)    # v: 현재 좌표
        # 시작점이 종점인 경우
        if v == end:
            return count[v[0]][v[1]]
        
        # 델타 이동
        dij = [[-2, -1], [-1, -2], [1, -2], [2, -1], [2, 1], [1, 2], [-1, 2], [-2, 1]]
        for di, dj in dij:
            w = [v[0]+di, v[1]+dj]  # w: 주변 좌표
            if 0 <= w[0] < n and 0 <= w[1] < n and not count[w[0]][w[1]]:
                queue.append(w)
                count[w[0]][w[1]] = count[v[0]][v[1]] + 1
                
                # 종점이면 count 반환
                if w == end:
                    return count[w[0]][w[1]]


T = int(input())
for tc in range(T):
    l = int(input())    # 체스판 한 변의 길이
    knight_start = list(map(int, input().split()))  # 시작점
    knight_end = list(map(int, input().split()))    # 종점

    print(BFS(l, knight_start, knight_end))
```

