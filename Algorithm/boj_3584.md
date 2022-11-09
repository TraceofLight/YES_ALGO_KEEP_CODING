## BOJ 3584. 가장 가까운 공통 조상

### 문제 링크

[[BOJ 3584] 가장 가까운 공통 조상](https://www.acmicpc.net/problem/3584)

### 분류

그래프 이론, 트리, 깊이 우선 탐색, 최소 공통 조상

### 코드

```python
# 가장 가까운 공통 조상

import sys

input = sys.stdin.readline

# 케이스 갯수 및 출력 리스트 선언
testcase = int(input())
output = []

for _ in range(testcase):

    # 정점 갯수 입력
    vertex_number = int(input())

    # 부모 노드 정보를 담을 리스트 선언
    parent = [None for _ in range(vertex_number + 1)]

    # 정점 관계 정보 입력
    for _ in range(vertex_number - 1):
        vertex_a, vertex_b = map(int, input().split())
        parent[vertex_b] = vertex_a

    # 확인할 두 노드 입력
    target_a, target_b = map(int, input().split())

    # 부모 정보를 확인할 스택 선언
    stack_a = []
    stack_b = []

    # 부모 관계를 거슬러 올라가서 루트를 만날 때까지 반복
    while target_a is not None:
        stack_a.append(target_a)
        target_a = parent[target_a]
    while target_b is not None:
        stack_b.append(target_b)
        target_b = parent[target_b]

    # 결과값 변수 선언
    result = None

    # 가장 가까운 공통 조상 확인
    while True:

        # 스택 최상단에서 뽑아서 조사
        check_parent_a = stack_a.pop()
        check_parent_b = stack_b.pop()

        # 두 값이 값다면 결과값 후보
        if check_parent_a == check_parent_b:
            result = check_parent_a

        # 같지 않을 경우 가장 최근에 선택한 결과값 후보가 가장 가까운 공통 조상
        else:
            break

        # 스택에 남은 게 없을 경우도 반복 종료, 직전 결과값이 가장 가까운 공통 조상
        if not stack_a or not stack_b:
            break

    # 결과값 출력 리스트에 추가
    output.append(result)

# 출력
for result in output:
    print(result)

```