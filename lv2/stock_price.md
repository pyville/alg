# 주식 가격

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

## 제한 사항

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

## 예제

| prices | return |
| --- | --- |
| [1, 2, 3, 2, 3] | [4, 3, 1, 1, 0] |

- 1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.
- 2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.
- 3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.
- 4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.
- 5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 스택을 사용합니다.
    - answer 리스트의 초기값은 가격이 끝까지 안 떨어졌을 때를 가정한 값입니다. 예: [4, 3, 2, 1, 0]
    - 기본적으로 순서대로 스택에 추가합니다.
    - prices[i]의 값이 기존에 스택에 있는 값보다 작은 경우, prices[i]보다 큰 값들을 모두 빼내고 answer 리스트에 가격이 떨어진 것으로 기록합니다.

```python
def solution(prices):
    stack = []
    answer = [len(prices)-1-i for i in range(len(prices))]
    for i in range(len(prices)):
        if stack != [] and prices[i] < stack[-1][1]:
            while stack != [] and prices[i] < stack[-1][1]:
                idx, _ = stack.pop()
                answer[idx] = i - idx
        stack.append((i, prices[i]))
    
    return answer
```