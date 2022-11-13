## BOJ 2004. 조합 0의 개수

### 문제 링크

[[BOJ 2004] 조합 0의 개수](https://www.acmicpc.net/problem/2004)

### 분류

수학, 정수론

### 코드

``` python
# from math import factorial

def num_of_five(num):
    pow5s = 5
    cnt_5 = 0
    while pow5s <= num:
        cnt_5 += num // pow5s
        pow5s = pow5s * 5

    return cnt_5

def num_of_two(num):
    pow2s = 2
    cnt_2 = 0
    while pow2s <= num:
        cnt_2 += num // pow2s
        pow2s = pow2s * 2
    
    return cnt_2


def nmnm(n, m):
    ans_5 = num_of_five(n) - num_of_five(n-m) - num_of_five(m)
    ans_2 = num_of_two(n) - num_of_two(n-m) - num_of_two(m)
    return min(ans_5, ans_2)
    

n, m = map(int,input().split())

print(nmnm(n, m))
```