## BOJ 2469. 사다리 타기

### 문제 링크

[[BOJ 2469] 사다리 타기](https://www.acmicpc.net/problem/2469)

### 분류

구현, 문자열

### 코드

``` python
def wh_question():
    for y_idx in range(row):
        if ladder[y_idx][1] == '?':
            return y_idx


def down(above_que: int):
    for idx in range(K):
        x_idx = idx
        for y_idx in range(above_que):
            if ladder[y_idx][x_idx + 1] == '-':
                x_idx += 1
            elif ladder[y_idx][x_idx] == '-':
                x_idx -= 1
        ball[x_idx] = chr(idx + 65)


def up(que: int):
    for idx in range(K):
        x_idx = idx
        for y_idx in range(row-1, que, -1):
            if ladder[y_idx][x_idx + 1] == '-':
                x_idx += 1
            elif ladder[y_idx][x_idx] == '-':
                x_idx -= 1
        hole[x_idx] = idx


K = int(input())
row = int(input())
result = input()
ladder = []
for _ in range(row):
    ladder.append('*' + input() + '*')

# ???인 줄을 찾기
question = wh_question()
ball = [0 for _ in range(K)]
hole = [0 for _ in range(K+1)]
ans = ['' for _ in range(K)]

# 내려보내기
down(question)

# 올라오기
up(question)

# 정답 정하기
for idx in range(K):
    if ball[idx] == result[hole[idx]]:
        ans[idx] = '*'
    elif ball[idx] == result[hole[idx + 1]]:
        ans[idx + 1] = '-'
    elif ball[idx] == result[hole[idx - 1]]:
        ans[idx] = '-'
    else:
        ans = 'x' * (K+1)
        break

# 출력
for char_idx in range(1, K):
    if ans[char_idx] == '':
        print('*', end='')
    else:
        print(ans[char_idx], end='')
```