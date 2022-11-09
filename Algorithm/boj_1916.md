## BOJ 1916. 최소비용 구하기

### 문제 링크

[[BOJ 1916] 최소비용 구하기](https://www.acmicpc.net/problem/1916)

### 분류

그래프 이론, 데이크스트라

### 코드

``` python
# 최소비용 구하기

import sys
from heapq import *

input = sys.stdin.readline

# 도시의 갯수 및 간선의 갯수 input
city_number = int(input())
line_number = int(input())

# 간선 정보를 기록할 딕셔너리 선언
graph = {idx: [] for idx in range(1, city_number + 1)}

# 간선 정보 기록
for _ in range(line_number):
    start_city, end_city, cost = map(int, input().split())
    graph[start_city].append([cost, end_city])

# 출발지, 목적지 input
depart, arrive = map(int, input().split())

# 출발지에서 다른 노드까지 거리를 나타낸 리스트 선언
distance = [2000000000 for _ in range(city_number + 1)]

# 우선순위 큐 선언 및 초기값 설정
priority_que = []
heappush(priority_que, [0, depart])

# 시작점 돌아오는 최단거리 0으로 변경
distance[depart] = 0


'''
이하 다익스트라 알고리즘 실행
임의의 노드로 가는 최단거리 간선에 대해 조사
해당 노드로부터 갈 수 있는 노드에 대해 최소값 갱신
'''


while priority_que:
    now_cost, way_point = heappop(priority_que)
    for destination_info in sorted(graph[way_point]):
        next_cost, destination = destination_info
        if distance[destination] > distance[way_point] + next_cost:
            distance[destination] = distance[way_point] + next_cost
            heappush(priority_que, [
                distance[destination], destination])

# 결과 출력
print(distance[arrive])

```

