# 문제

땅따먹기 게임을 하려고 합니다. 땅따먹기 게임의 땅(land)은 총 N행 4열로 이루어져 있고, 모든 칸에는 점수가 쓰여 있습니다. 1행부터 땅을 밟으며 한 행씩 내려올 때, 각 행의 4칸 중 한 칸만 밟으면서 내려와야 합니다. 

**단, 땅따먹기 게임에는 한 행씩 내려올 때, 같은 열을 연속해서 밟을 수 없는 특수 규칙이 있습니다.**

예를 들면,

| 1 | 2 | 3 | 5 |
| - | - | - | - |
| 5 | 6 | 7 | 8 |
| 4 | 3 | 2 | 1 |

로 땅이 주어졌다면, 1행에서 네번째 칸 (5)를 밟았으면, 2행의 네번째 칸 (8)은 밟을 수 없습니다.

마지막 행까지 모두 내려왔을 때, 얻을 수 있는 점수의 최대값을 return하는 solution 함수를 완성해 주세요. 위 예의 경우, 1행의 네번째 칸 (5), 2행의 세번째 칸 (7), 3행의 첫번째 칸 (4) 땅을 밟아 16점이 최고점이 되므로 16을 return 하면 됩니다.

## 제한사항

- 행의 개수 N : 100,000 이하의 자연수
- 열의 개수는 4개이고, 땅(land)은 2차원 배열로 주어집니다.
- 점수 : 100 이하의 자연수

## 예제

| land | answer | 
| [[1,2,3,5],[5,6,7,8],[4,3,2,1]] | 16 |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 동적 계획법을 이용합니다.
    - 현재 행까지 진행했을 때 얻을 수 있는 점수의 최댓값 (마지막으로 밟는 칸 별로 4개)은 {이전 행까지 진행했을 때 얻을 수 있는 점수의 최댓값 (마지막으로 밟는 칸 별로 4개)} 및 {현재 행의 각 칸별 점수}를 연산하여 얻을 수 있는 점수의 최댓값입니다.
    - 먼저 1행과 2행의 값을 하나씩 뽑아 더할 수 있는 모든 경우의 수를 연산합니다. 같은 줄을 연속으로 밟을 수 없으므로, 경우의 수는 16-4=12가지입니다.
    - 마지막으로 밟은 줄을 기준으로 최대치만 뽑아서 경우의 수를 4가지로 좁힙니다.
    - 이렇게 만든 4가지 경우와 3행의 값을 하나씩 뽑아 더할 수 있는 모든 경우의 수를 연산합니다. 마찬가지로 같은 줄을 연속으로 밟을 수 없으므로, 경우의 수는 16-4=12가지입니다.
    - 마지막 행까지 반복하면 최대로 얻을 수 있는 점수를 구할 수 있습니다.

```python
# 12개의 값을 마지막 인덱스에 따른 최댓값 4개로 추려 줌
def shrink(arr):
    arr.sort(key=lambda x: x[0], reverse=True)
    q = []
    indices = []
    for value, index in arr:
        if index not in indices:
            q.append((value, index))
            indices.append(index)
    return q

def solution(land):
    if len(land) == 1:
        return max(land)
    
    old = [(value, index) for index, value in enumerate(land[0])]
    
    for land_new in land[1:]:
        tmp = []
        new = [(value, index) for index, value in enumerate(land_new)]
        for old_value, old_index in old:
            for new_value, new_index in new:
                if old_index != new_index:
                    tmp.append((old_value + new_value, new_index))
        old = shrink(tmp)
    return old[0][0]
```