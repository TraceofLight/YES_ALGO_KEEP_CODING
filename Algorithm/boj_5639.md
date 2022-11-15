## BOJ 5639. 이진 검색 트리

### 문제 링크

[[BOJ 5639] 이진 검색 트리](https://www.acmicpc.net/problem/5639)

### 분류

트리, 재귀, 그래프 이론, 그래프 탐색

### 코드

``` python
import sys
sys.setrecursionlimit(10**5)


def postorder(start, end):
    if start > end:
        return
    mid = end + 1  # 루트보다 큰 값이 존재하지 않을 경우를 대비
    for i in range(start + 1, end + 1):
        if bin_search[start] < bin_search[i]:
            mid = i
            break

    postorder(start + 1, mid - 1)
    postorder(mid, end)
    print(bin_search[start])


bin_search = []

while True:
    try:
        new_node = int(input())
        bin_search.append(new_node)
    except EOFError:
        break

postorder(0, len(bin_search) - 1)
```