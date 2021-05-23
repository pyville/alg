# 더 맵게

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

> 섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다. Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

## 예제

| scoville | K | return |
| --- | --- | --- |
| [1, 2, 3, 9, 10, 12] | 7 | 2 |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 우선순위 큐를 사용합니다.

```python
import heapq

def solution(scoville, K):
    answer = 0
    heap = []
		# 우선순위 큐에 모든 음식 입력
		# heapq.heapify(scoville) 과 같은 코드
    for item in scoville:
        heapq.heappush(heap, item)
    
		# while문을 돌면서 새로운 음식 만들기
    while heap[0] < K:
        if len(heap) == 1: break # 음식이 하나 남았을 때는 빠져나오기
				# 우선순위 큐에서 2개를 빼내서 스코빌 지수 공식에 따라 새로운 음식 만들고 다시 우선순위 큐에 넣기
        heapq.heappush(heap, heapq.heappop(heap)+heapq.heappop(heap)*2)
				# 횟수 추가
        answer += 1
    
		# while문이 끝났는데 여전히 스코빌 지수가 K보다 작을 경우 -1 반환
    if heap[0] < K: return -1
    
    return answer
```