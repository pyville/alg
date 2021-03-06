# 가장 큰 정사각형 찾기

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/12905)

[돌아가기](/../alg/)

1와 0로 채워진 표(board)가 있습니다. 표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다. 표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요. (단, 정사각형이란 축에 평행한 정사각형을 말합니다.)

예를 들어

| 1 | 2 | 3 | 4 |
| - | - | - | - |
| 0 | 1 | 1 | 1 |
| 1 | 1 | 1 | 1 |
| 1 | 1 | 1 | 1 |
| 0 | 0 | 1 | 0 |

가 있다면 가장 큰 정사각형은

| 1 | 2 | 3 | 4 |
| - | - | - | - |
| 0 | [1] | [1] | [1] |
| 1 | [1] | [1] | [1] |
| 1 | [1] | [1] | [1] |
| 0 | 0 | 1 | 0 |

가 되며 넓이는 9가 되므로 9를 반환해 주면 됩니다.

## 제한사항

- 표(board)는 2차원 배열로 주어집니다.
- 표(board)의 행(row)의 크기 : 1,000 이하의 자연수
- 표(board)의 열(column)의 크기 : 1,000 이하의 자연수
- 표(board)의 값은 1또는 0으로만 이루어져 있습니다.

## 예제

| land | answer |
| --- | --- |
| [[0,1,1,1],[1,1,1,1],[1,1,1,1],[0,0,1,0]] | 9 |
| [[0,0,1,1],[1,1,1,1]] | 4 |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 동적 계획법을 이용합니다.
    - 먼저, 각 정사각형의 위치를 오른쪽 아래로 보았을 때 가장 큰 정사각형의 크기를 저장하는 2차원 리스트를 하나 만듭니다.
    - 기본적으로 이 리스트는 표의 값이 0인 위치에서는 똑같이 0을 가집니다.
    - 리스트의 첫 행에서는 각 정사각형의 위치를 오른쪽 아래로 보았을 때 최대 1 크기로만 나올 수 있으므로 1일 때 1, 0일 때 0을 저장합니다.
    - 리스트의 첫 열도 마찬가지로 지정합니다.
    - 리스트의 두번째 행, 두번째 열부터는 자신을 기준으로 리스트의 왼쪽, 위 및 왼쪽 위 대각선 값의 최솟값에 1을 더한 값이 자신을 오른쪽 아래로 보았을 때 가장 큰 정사각형의 가로 또는 세로 길이가 됩니다.
    - 2차원 리스트에서 최댓값을 계산하기 번거롭기 때문에, 최댓값은 그때그때 저장해 둡니다.

```python
def solution(board):
		# 모든 값이 0인 2차원 리스트 생성
    max_mem = [[0 for a in range(len(board[0]))] for b in range(len(board))]
		# 첫 행에 값 지정
    max_mem[0] = [1 if board[0][i] == 1 else 0 for i in range(len(board[0]))]
    
    max_value = max(max_mem[0])
    
		# 첫 열에 값 지정
    for j in range(len(max_mem)):
        if board[j][0] == 1:
            max_mem[j][0] = 1
            max_value = 1
        else:
            max_mem[j][0] = 0
    
	  # 두번째 행, 두번째 열부터 연산
    for x in range(len(max_mem)):
        for y in range(len(max_mem[x])):
						# 첫번째 행 또는 첫번째 열에 속하거나 
						# 해당 위치의 값이 0일 때는 그냥 0으로 놔둠
            if x == 0 or y == 0 or board[x][y] != 1:
                continue
            
						# 자신의 왼쪽, 위, 왼쪽 위의 값에 따른 값 지정
            max_mem[x][y] = min(max_mem[x-1][y], max_mem[x][y-1], max_mem[x-1][y-1]) + 1

            # 최댓값 갱신
            if max_mem[x][y] > max_value:
                max_value = max_mem[x][y]
    return max_value ** 2
```