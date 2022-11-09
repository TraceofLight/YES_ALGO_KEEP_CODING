## BOJ 2580. 스도쿠

### 문제 링크

[[BOJ 2580] 스도쿠](https://www.acmicpc.net/problem/2580)

### 분류

백트래킹

### 코드

```python
# 스도쿠 

import sys

input = sys.stdin.readline


def is_make_row(total_map: list, target_row: int, target_number: int) -> bool:
    '''
    스도쿠의 행에 대해서 조건을 만족하는지 확인하는 함수
    '''

    # 만약 해당 행에서 중복인 숫자가 발생한다면 False 반환
    for idx in range(9):
        if total_map[target_row][idx] == target_number:
            return False

    # 발생하지 않았다면 True 반환
    return True

def is_make_column(total_map: list, target_col: int, target_number: int) -> bool:
    '''
    스도쿠의 열에 대해서 조건을 만족하는지 확인하는 함수
    '''

    # 만약 해당 열에서 중복인 숫자가 발생한다면 False 반환
    for idx in range(9):
        if total_map[idx][target_col] == target_number:
            return False

    # 발생하지 않았다면 True 반환
    return True

def is_make_box(total_map: list, now_idx: list, target_number: int) -> bool:
    '''
    스도쿠의 3 * 3 기준 박스에 대해서 조건을 만족하는 확인하는 함수
    '''

    # 좌표 x, y 성분으로 분해
    now_y, now_x = now_idx

    # 기준점 변수 선언
    start_y, start_x = (now_y // 3) * 3, (now_x // 3) * 3

    # 기준점에서부터 3 * 3 범위 조사하면서 중복이 발생했다면 False 반환
    for add_y in range(3):
        for add_x in range(3):
            if total_map[start_y + add_y][start_x + add_x] == target_number:
                return False

    # 발생하지 않았다면 True 반환
    return True

def make_sudoku(target_sudoku: list, blank_list: list, now_blank: int, target_blank: int) -> None:
    '''
    스도쿠의 빈칸을 채워서 가능한 조합 중 사전순으로 가장 앞에 존재하는 스도쿠를 완성하는 함수
    '''

    # 모든 빈칸을 채웠다면 해당 스도쿠 리스트를 반환
    if now_blank == target_blank:
        return target_sudoku

    # 아직 모자란 상태라면 백트래킹 진행
    elif now_blank <= target_blank:

        # 현재 목표로 하는 칸 좌표 확인
        check_idx = blank_list[now_blank]
        check_y, check_x = check_idx

        # 1부터 9까지 숫자에 대해 조사
        for check_number in range(1, 10):

            # 스도쿠 조건에 부합할 경우
            if (
                is_make_row(target_sudoku, check_y, check_number)
                and is_make_column(target_sudoku, check_x, check_number)
                and is_make_box(target_sudoku, check_idx, check_number)
            ):
                # 해당 칸에 숫자 채우기
                target_sudoku[check_y][check_x] = check_number

                # 함수를 재귀 호출하여 이어서 다음 빈칸으로 진행 
                now_result = make_sudoku(target_sudoku, blank_list, now_blank + 1, target_blank)

                # 결과값을 1번 보고 온 경우 해당 값을 그대로 계속 반환
                if now_result:
                    return now_result

                # 유망하지 않았을 경우 다시 0으로 되돌리기
                target_sudoku[check_y][check_x] = 0

        # 1부터 9까지 조사 후 적합한 케이스가 없었다면 False 반환
        return False

# 미완성 스도쿠 및 빈칸 정보를 담을 리스트 선언
be_sudoku = []
blanks = []

# 스도쿠 정보 입력
for y_idx in range(9):

    # 행 정보 입력
    temp = list(map(int, input().split()))

    # 빈칸이 존재한다면 해당 인덱스 정보 리스트에 추가
    for x_idx in range(9):
        if not temp[x_idx]:
            blanks.append((y_idx, x_idx))
    
    # 행 정보를 스도쿠 리스트에 추가
    be_sudoku.append(temp)

# 함수를 호출하여 완성된 스도쿠 결과 확인
complete_sudoku = make_sudoku(be_sudoku, blanks, 0, len(blanks))

# 출력
for row in complete_sudoku:
    print(*row)
```