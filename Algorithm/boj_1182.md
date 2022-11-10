## BOJ 1182. 부분수열의 합

### 문제 링크

[[BOJ 1182\] 부분수열의 합](https://www.acmicpc.net/problem/1182)

### 분류

브루트포스 알고리즘, 백트래킹

### 코드

```python
N, S = map(int, input().split())
nums = list(map(int, input().split()))

count = 0
# i: 부분수열의 개수만큼 순회
for i in range(1, 1 << N):
    sum_subset = 0
    
    # j: 수열의 원소 개수만큼 순회
    for j in range(N):
        # 각 원소가 부분순열에 포함되어있는지에 따라, sum에 더함
        if i & (1 << j):
            sum_subset += nums[j]
    
    # 부분수열 합이 S이면, 카운트 횟수 +1
    if sum_subset == S:
        count += 1
print(count)
```

