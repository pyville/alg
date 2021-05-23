# 문제

회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다. 야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.

## 제한사항

- `works`는 길이 1 이상, 20,000 이하인 배열입니다.
- `works`의 원소는 50000 이하인 자연수입니다.
- `n`은 1,000,000 이하인 자연수입니다.

## 예제

| works | n | result |
| - | - | - |
| [4, 3, 3] | 4 | 12 |
| [2, 1, 2] | 1 | 6 |
| [1, 1] | 3 | 0 |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 탐욕법, 우선순위 큐를 이용합니다.
    - 이 문제에서는 매 시간마다 가장 많이 남은 일에 작업량을 할당하는 것을 반복하면 최적의 해가 나옵니다.
    - 하지만 매번 가장 많이 남은 일을 찾기 위해서는 정렬이 필요하므로, 최솟값(최댓값)이 먼저 출력되는 우선순위 큐를 이용합니다.

```python
import heapq

def solution(n, works):
    if sum(works) <= n:
        return 0
    
    heap = []

		# 모든 값을 heap에 저장 (단, heapq는 최솟값이 먼저 출력되므로 -기호 붙임)
    for work in works:
        heapq.heappush(heap, -work)

    # heap에서 최댓값을 꺼내서 하나 빼서 다시 넣는 작업을 n번 수행
    for i in range(n):
        heapq.heappush(heap, heapq.heappop(heap)+1)
    
		# 남은 일에 대한 야근 지수 계산
    answer = sum(map(lambda x: x ** 2, heap))
    return answer
```