## BOJ 2193. 이친수

### 문제 링크

[[BOJ 2193] 이친수](https://www.acmicpc.net/problem/2193)

### 분류

다이나믹 프로그래밍

### 코드

```python
import sys

number = int(sys.stdin.readline())
val_list = [0, 1, 1]


def pinary_number(val):
    global val_list
    length = len(val_list)
    if val <= length - 1:
        return val_list[val]
    else:
        for idx in range(length, val + 1):
            val_list.append(val_list[idx - 2] + sum(val_list[:idx - 2]) + 1)
    return val_list[val]


print(pinary_number(number))
```