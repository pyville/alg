# 타겟 넘버

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/43165)

[돌아가기](/../alg/)

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항

- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

## 예제

| numbers | target | return |
| --- | --- | --- |
| [1, 1, 1, 1, 1] | 3 | 5 |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- DFS, 재귀를 사용합니다.
    - root부터 - 또는 + 부호를 선택하여 주어진 수의 개수만큼 뻗어나가는 이진 트리 구조를 생각하면 됩니다.
    - 값들은 0에서 시작하여 numbers에 주어진 값들을 각 부호에 맞게 더하거나 뺍니다.
    - 최종적으로 leaf node에 도달한 경우의 값이 target과 같으면 answer에 1을 더합니다.

```python
answer = 0

def solution(numbers, target):
    max_depth = len(numbers)
    
    def traverse(value, depth):
				# answer는 함수 바깥에서 접근할 수 있도록 전역 변수로 설정
        global answer

				# leaf node에 도달한 경우, value가 target과 같으면 answer에 1을 더함
        if depth == max_depth:
            if value == target: answer += 1
            return True

				# 다음 부호가 -인 경우와 +인 경우에 대해 재귀 실행
        traverse(value-numbers[depth], depth+1)
        traverse(value+numbers[depth], depth+1)
        
    traverse(0, 0)
    return answer
```