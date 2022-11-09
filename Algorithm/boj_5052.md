## BOJ 5052. 전화번호 목록

### 문제 링크

[[BOJ 5052] 전화번호 목록](https://www.acmicpc.net/problem/5052)

### 분류

자료 구조, 문자열, 트라이

### 코드

```python
# 전화번호 목록

import sys

input = sys.stdin.readline

# Trie Algorithm

# 각 정점의 정보를 입력하는 노드 클래스 선언
class Node():

    def __init__(self, key):

        # 노드가 가져야 하는 값들을 파라미터로 보유
        self.key = key
        self.data = None
        self.children = dict()


# 트라이 알고리즘 사용을 위한 트라이 클래스 선언
class Trie():

    # 선언 시에 루트 노드는 자동으로 선언
    def __init__(self) -> None:
        self.root = Node('root')

    # 노드를 추가하는 셀프 인스턴스
    def add(self, string: str) -> None:

        # 루트에서 시작
        now_node = self.root

        # 모든 문자열에 대해 조사
        for chr in string:

            # 다음 문자에 대한 노드가 존재하지 않을 경우 노드를 생성
            if now_node.children.get(chr) is None:
                now_node.children[chr] = Node(chr)

            # 다음 노드로 이동
            now_node = now_node.children[chr]

        # 마지막 노드의 데이터값에 문자열 전체를 기록
        now_node.data = string

    # 일관성을 유지하는지 확인하는 셀프 인스턴스
    def is_interrupt(self, target_string: str) -> bool:

        # 루트에서 시작
        now_node = self.root

        # 모든 문자열에 대해 조사
        for chr in target_string:

            # 문자열의 마지막 노드가 아닌데 데이터가 존재하는 경우 일관성을 유지하지 못함
            if now_node.data is not None and now_node.data != target_string:
                return True

            # 다음 노드로 이동
            now_node = now_node.children[chr]

        # 모든 노드에 대해서 일관성 체크가 되었다면 False를 반환
        return False


# 케이스 횟수 및 출력 리스트 선언
testcase = int(input())
output = []

for _ in range(testcase):

    # 전화번호 갯수 입력
    number_amount = int(input())

    # 트라이 객체 선언 및 전화번호 리스트 선언
    phone_trie = Trie()
    number_list = []

    # 번호를 트라이 객체 및 리스트에 입력
    for _ in range(number_amount):
        each_number = input().rstrip('\n')
        phone_trie.add(each_number)
        number_list.append(each_number)

    # 일관성 유지에 대한 변수 선언
    is_overlapped = False

    # 모든 전화번호에 대해서 일관성을 유지하고 있는지 확인
    for phone_number in number_list:

        # 일관성을 유지하지 못했다면 True로 변경 및 반복문 break
        if phone_trie.is_interrupt(phone_number):
            is_overlapped = True
            break

    # 조건에 따라 출력 리스트에 결과를 추가
    if is_overlapped:
        output.append('NO')
    else:
        output.append('YES')

# 출력
for result in output:
    print(result)

```