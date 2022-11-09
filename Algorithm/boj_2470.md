## BOJ 2470. 두 용액

### 문제 링크

[[BOJ 2470] 두 용액](https://www.acmicpc.net/problem/2470)

### 분류

투 포인터

### 코드

```python
# 두 용액

import sys

input = sys.stdin.readline

# 용액 갯수 및 용액 리스트 input
liquid_number = int(input())
liquid_list = list(map(int, input().split()))

# 특성값 순으로 정렬
sorted_list = sorted(liquid_list)

# 투 포인터 알고리즘 사용
first_cursor = 0
second_cursor = liquid_number - 1

# 최소 특성값 변수 선언 및 최소값인 경우의 두 용액을 저장할 변수 선언
min_sum = 2000000000
two_liquid = None

# 포인터가 겹칠 때까지 반복
while first_cursor != second_cursor:

    # 포인터가 가리키는 두 용액 합산
    result = sorted_list[first_cursor] + sorted_list[second_cursor]

    # 최소 특성값을 갱신했다면 기록
    if min_sum > abs(result):
        min_sum = abs(result)
        two_liquid = [sorted_list[first_cursor], sorted_list[second_cursor]]

        # 만약 0이라면 갱신이 더 발생하지 않으므로 거기서 반복 종료
        if not result:
            break
    
    # 값이 양수라면 뒷 커서를 한 칸 당기기
    if result > 0:
        second_cursor -= 1
    
    # 값이 음수라면 앞 커서를 한 칸 밀기
    elif result < 0:
        first_cursor += 1

# 최소 특성값의 두 용액을 출력
print(*two_liquid)

```