## BOJ 9935. 문자열 폭발

### 문제 링크

[[BOJ 9935] 문자열 폭발](https://www.acmicpc.net/problem/9935)

### 분류

자료 구조, 문자열, 스택

### 코드

```python
# 문자열 폭발

import sys
from collections import deque

input = sys.stdin.readline

# 문자열, 폭발 문자열 input
string = deque(list(input().rstrip('\n')))
bomb = deque(list(input().rstrip('\n')))

# 중간에 폭발 문자열을 체크할 큐 및 길이 변수 선언
progress_que = deque([])
que_length = 0

# 문자열 길이 및 폭탄 문자열 길이 선언
total_length = len(string)
bomb_length = len(bomb)

# 검사가 완료된 문자열을 넣을 스택 선언
stack1 = []

# 시작부터 폭발 문자열보다 작은 문자열을 받을 경우 그대로 출력
if total_length < bomb_length:
    print(''.join(string))

else:
    # 아직 체크하지 않은 문자열이 남아있는 동안 진행
    while string or progress_que:

        # 폭탄 문자열이랑 현재 커서 문자열이 길이가 같을 때까지 string에서 내릴 것
        while string and que_length < bomb_length:
            progress_que.append(string.popleft())
            que_length += 1

        # 만약 문자열이 같다면 해당 문자열을 제거 후 앞에서 폭탄 문자열보다 1칸 작게 다시 올릴 것
        if progress_que == bomb:
            progress_que.clear()
            que_length = 0
            while stack1 and que_length < bomb_length - 1:
                progress_que.appendleft(stack1.pop())
                que_length += 1

        # 문자열이 다르다면 1칸 내리고 새로 받을 수 있는 경우 1칸 받을 것
        elif progress_que and progress_que != bomb:
            stack1.append(progress_que.popleft())
            que_length -= 1
            if string:
                progress_que.append(string.popleft())
                que_length += 1

        # string이 전부 정리된 경우 문자열 체크가 불가능하다면 전부 밑으로 내리기
        elif not string and que_length < bomb_length:
            while progress_que:
                stack1.append(progress_que.popleft())
            que_length = 0
            break

    # 결과 출력
    result = ''.join(stack1)

    # 전부 폭발한 경우 FRULA 출력
    if result == '':
        print('FRULA')
    else:
        print(result)

```