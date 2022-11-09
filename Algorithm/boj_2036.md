## BOJ 2036. 수열의 점수

### 문제 링크

[[BOJ 2036] 수열의 점수](https://www.acmicpc.net/problem/2036)

### 분류

그리디 알고리즘, 많은 조건 분기

### 코드

```python
# 수열의 점수

import sys
from bisect import *

input = sys.stdin.readline


def max_score(target_list: list, length: int) -> int:
    '''
    정렬된 리스트에서 나올 수 있는 점수의 최대값을 반환하는 함수
    '''

    # 원소가 하나일 경우
    if length == 1:

        # 해당 원소의 값을 반환
        return target_list[0]

    # 원소가 하나가 아닐 경우
    else:

        # 결과값 변수 선언
        result = 0

        # 0을 기준으로 좌우 인덱스 구분
        zero_index_left = bisect_left(target_list, 0)
        zero_index_right = bisect_right(target_list, 0)

        # 0의 갯수 확인
        zero_count = zero_index_right - zero_index_left

        # 좌측 포인터 정렬
        left_counter = 0

        # 0 위치 직전까지 조사
        while left_counter < zero_index_left:

            # 음수 2개를 곱할 수 있는 경우 둘의 곱을 합산
            if left_counter + 1 < zero_index_left:
                result += target_list[left_counter] * target_list[left_counter + 1]

                # 포인터 2 이동
                left_counter += 2

            # 음수가 1개만 남은 경우
            else:

                # 곱할 0이 존재하지 않는 경우
                if not zero_count:

                    # 음수값을 합산
                    result += target_list[left_counter]

                # 포인터 1 이동
                left_counter += 1

        # 우측 맨 끝에서부터 조사
        right_counter = length - 1

        # 0 위치 직전까지 조사
        while right_counter >= zero_index_right:

            # 수를 곱할 수 있는 경우
            if right_counter - 1 >= zero_index_right:

                # 만약 해당 수 직전 수가 1이라면
                if target_list[right_counter - 1] == 1:

                    # 곱한 것보다 더한 값이 크다
                    result += target_list[right_counter] + target_list[right_counter - 1]

                # 1이 아니라면
                else:

                    # 곱한 값이 더 크다
                    result += target_list[right_counter] * target_list[right_counter - 1]

                # 포인터 2 이동
                right_counter -= 2

            # 곱할 수 없는 경우
            else:

                # 남은 수를 합산
                result += target_list[right_counter]

                # 포인터 1 이동
                right_counter -= 1

        # 결과 반환
        return result


# 수열 원소의 갯수 입력
number_amount = int(input())

# 수열 정보 입력
number_list = []
for _ in range(number_amount):
    number_list.append(int(input()))

# 크기 순으로 정렬
number_list.sort()

# 함수를 호출하여 결과 확인 후 출력
print(max_score(number_list, number_amount))

```