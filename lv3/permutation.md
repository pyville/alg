# 줄 서는 방법

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/12936)

[돌아가기](/../alg/)

n명의 사람이 일렬로 줄을 서고 있습니다. n명의 사람들에게는 각각 1번부터 n번까지 번호가 매겨져 있습니다. n명이 사람을 줄을 서는 방법은 여러가지 방법이 있습니다. 예를 들어서 3명의 사람이 있다면 다음과 같이 6개의 방법이 있습니다.

- [1, 2, 3]
- [1, 3, 2]
- [2, 1, 3]
- [2, 3, 1]
- [3, 1, 2]
- [3, 2, 1]

사람의 수 n과, 자연수 k가 주어질 때, 사람을 나열 하는 방법을 사전 순으로 나열 했을 때, k번째 방법을 return하는 solution 함수를 완성해주세요.

## 제한 사항

- n은 20이하의 자연수입니다.
- k는 n! 이하의 자연수입니다.

## 예제

| n | k | result |
| - | - | - |
| 3 | 5 | [3,1,2] |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 완전 탐색을 사용합니다.
- n! * (n-1)! * ... * 1개의 가짓수를 가지는 순열의 특성을 이용합니다.

```python
def factorial(n):
    if n in [0,1]: return 1
    return factorial(n-1)*n

def solution(n, k):
    candidate = [i+1 for i in range(n)]
    answer = []
    fac = factorial(n)
    for i in range(1,n+1):
        fac = fac // (n-i+1)
        if k % fac == 0:
            answer.append(candidate.pop(k // fac - 1))
            return answer + list(reversed(candidate))
        tmp = (k // fac) + 1
        answer.append(candidate.pop(tmp-1))
        k -= (k // fac) * fac
    answer += candidate
    return answer
```