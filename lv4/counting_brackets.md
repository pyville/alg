# 올바른 괄호의 개수

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/12929)

[돌아가기](/../../)

올바른 괄호란 (())나 ()와 같이 올바르게 모두 닫힌 괄호를 의미합니다. )(나 ())() 와 같은 괄호는 올바르지 않은 괄호가 됩니다. 괄호 쌍의 개수 n이 주어질 때, n개의 괄호 쌍으로 만들 수 있는 모든 가능한 괄호 문자열의 갯수를 반환하는 함수 solution을 완성해 주세요.

## 제한 사항

- 괄호 쌍의 개수 N : 1 ≤ n ≤ 14, N은 정수

## 예제

| n | result |
| - | - |
| 2 | 2 |
| 3 | 5 |

입출력 예 #1

2개의 괄호쌍으로 [ "(())", "()()" ]의 2가지를 만들 수 있습니다.

입출력 예 #2

3개의 괄호쌍으로 [ "((()))", "(()())", "(())()", "()(())", "()()()" ]의 5가지를 만들 수 있습니다.

## 풀이

python을 이용한 풀이는 아래와 같습니다.

- 데크(deque)를 사용합니다.

```python
from collections import deque

def solution(n):
    deq = deque()
    deq.append(('(', 1, 0))
    ans = 0
    while len(deq) != 0:
        s, left, right = deq.popleft()
        
        if len(s) == n*2:
            if left == right: ans += 1
            continue
        if left < n: deq.append((s+'(', left+1, right))
        if left > right: deq.append((s+')', left, right+1))
            
    return ans
```