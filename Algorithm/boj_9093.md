## BOJ 9093. 단어 뒤집기

### 문제 링크

[[BOJ 9093\] 단어 뒤집기](https://www.acmicpc.net/problem/9093)

### 분류

구현, 문자열

### 코드

```python
T = int(input())

for tc in range(T):
    sentence = list(input().split())
    reversed_words = []
    for word in sentence:
        reversed_words.append(word[::-1])
    print(' '.join(reversed_words))
```

