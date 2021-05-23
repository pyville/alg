# 행렬 테두리 회전하기

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/77485)

[돌아가기](/../alg/)

rows x columns 크기인 행렬이 있습니다. 행렬에는 1부터 rows x columns까지의 숫자가 한 줄씩 순서대로 적혀있습니다. 이 행렬에서 직사각형 모양의 범위를 여러 번 선택해, 테두리 부분에 있는 숫자들을 시계방향으로 회전시키려 합니다. 각 회전은 (x1, y1, x2, y2)인 정수 4개로 표현하며, 그 의미는 다음과 같습니다.

- x1 행 y1 열부터 x2 행 y2 열까지의 영역에 해당하는 직사각형에서 테두리에 있는 숫자들을 한 칸씩 시계방향으로 회전합니다.

다음은 6 x 6 크기 행렬의 예시입니다.

<img width="332" alt="grid_example" src="https://user-images.githubusercontent.com/52960121/119263898-e6f6b980-bc1b-11eb-95b0-d391f553a7ae.png">

이 행렬에 (2, 2, 5, 4) 회전을 적용하면, 아래 그림과 같이 2행 2열부터 5행 4열까지 영역의 테두리가 시계방향으로 회전합니다. 이때, 중앙의 15와 21이 있는 영역은 회전하지 않는 것을 주의하세요.

<img width="336" alt="rotation_example" src="https://user-images.githubusercontent.com/52960121/119263915-f7a72f80-bc1b-11eb-8572-cfac6013cade.png">

행렬의 세로 길이(행 개수) rows, 가로 길이(열 개수) columns, 그리고 회전들의 목록 queries가 주어질 때, 각 회전들을 배열에 적용한 뒤, 그 회전에 의해 위치가 바뀐 숫자들 중 가장 작은 숫자들을 순서대로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- rows는 2 이상 100 이하인 자연수입니다.
- columns는 2 이상 100 이하인 자연수입니다.
- 처음에 행렬에는 가로 방향으로 숫자가 1부터 하나씩 증가하면서 적혀있습니다.
    - 즉, 아무 회전도 하지 않았을 때, i 행 j 열에 있는 숫자는 ((i-1) x columns + j)입니다.
- queries의 행의 개수(회전의 개수)는 1 이상 10,000 이하입니다.
- queries의 각 행은 4개의 정수 [x1, y1, x2, y2]입니다.
    - x1 행 y1 열부터 x2 행 y2 열까지 영역의 테두리를 시계방향으로 회전한다는 뜻입니다.
    - 1 ≤ x1 < x2 ≤ rows, 1 ≤ y1 < y2 ≤ columns입니다.
    - 모든 회전은 순서대로 이루어집니다.
    - 예를 들어, 두 번째 회전에 대한 답은 첫 번째 회전을 실행한 다음, 그 상태에서 두 번째 회전을 실행했을 때 이동한 숫자 중 최솟값을 구하면 됩니다.

## 예제

| rows | columns | queries | result | 
| - | - | - | - |
| 6 | 6 | [[2,2,5,4],[3,3,6,6],[5,1,6,3]] | [8, 10, 25] |
| 3 | 3 | [[1,1,2,2],[1,2,2,3],[2,1,3,2],[2,2,3,3]] | [1, 1, 5, 3] |
| 100 | 97 | [[1,1,100,97]] | [1] |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 직접 행렬 테두리 회전을 구현합니다.
    - 1) 윗 행은 오른쪽으로 한 칸씩 밉니다. 이때 윗 행의 맨 오른쪽 칸은 2) 오른쪽 열로 이동하게 되므로 포함하지 않습니다. 이를 슬라이싱하여 row1로 저장해 둡니다.
    - 2) 오른쪽 열은 아래쪽으로 한 칸씩 밉니다. 이때 for 문이 거꾸로 돌아야 한다는 것에 유의합니다.
    - 3) 아랫 행은 왼쪽으로 한 칸씩 밉니다. 이때 아랫 행의 맨 윗쪽 칸은 4) 왼쪽 열로 이동하게 되므로 포함하지 않습니다. 이를 슬라이싱하여 row2로 저장해 둡니다.
    - 4) 왼쪽 열은 윗쪽으로 한 칸씩 밉니다.
    - 실제 작업 순서는 for 문 두 개를 먼저 수행한 후, row1과 row2에 저장된 값을 알맞은 위치에 넣어 주면 됩니다.
- 연산한 값 중에서 최솟값을 answer 리스트에 담습니다.

```python
def solution(rows, columns, queries):
    answer = []
		# 전체 행렬 만들기
    board = [[columns * j + (i+1) for i in range(columns)] for j in range(rows)]
		# 쿼리별 처리
    for query in queries:
				# 각 위치 a, b, c, d 지정 (인덱스는 0부터 시작하므로 -1 해줌)
        a, b, c, d = query[0]-1, query[1]-1, query[2]-1, query[3]-1

				# 슬라이싱으로 row1, row2 저장
        row1, row2 = board[a][b:d], board[c][b+1:d+1]

				# row1 및 row2에 있는 값들 중 최솟값으로 tmp 지정
        tmp = min(row1 + row2)
    
        for i in range(c, a, -1):
            board[i][d] = board[i-1][d]
						# tmp 갱신
            if board[i][d] < tmp: tmp = board[i][d]

        for i in range(a, c):
            board[i][b] = board[i+1][b]
						# tmp 갱신
            if board[i][b] < tmp: tmp = board[i][b]

				# 저장한 row1, row2 알맞은 위치에 넣어 줌
        board[a][b+1:d+1], board[c][b:d] = row1, row2

        # 최솟값 answer 리스트에 추가
        answer.append(tmp)
    return answer
```
