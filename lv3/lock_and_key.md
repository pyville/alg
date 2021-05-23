# 자물쇠와 열쇠

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/76502)

[돌아가기](/../alg/)

고고학자인  **"튜브"**는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 **자물쇠**로 잠겨 있었고 문 앞에는 특이한 형태의 **열쇠**와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가  **`1 x 1`**인  **`N x N`** 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 **`M x M`** 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 `true`를, 열 수 없으면 `false`를 return 하도록 solution 함수를 완성해주세요.

## 제한 사항

- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
    - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.

## 예제

| key | lock | result |
| - | - | - |
| [[0, 0, 0], [1, 0, 0], [0, 1, 1]] | [[1, 1, 1], [1, 1, 0], [1, 0, 1]] | true |

key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 완전 탐색을 사용합니다.
    - rotate는 key를 반시계 방향으로 90도 회전하는 함수입니다. 시계 방향으로 구현해도 상관없습니다.
    - challenge는 key를 오른쪽으로 a만큼, 아래로 b만큼 움직였을 때 꼭 맞는지 확인해 보는 함수입니다. 다른 방식으로 구현할 수도 있지만, 효율적인 알고리즘이 따로 있진 않아 보입니다.
    - solution 함수에서는 key를 총 4번 회전하고, 각각의 경우에 대해 key가 상하좌우로 움직일 수 있는 최대치만큼 challenge를 수행합니다.

```python
from copy import deepcopy

# key를 반시계 방향으로 90도 회전하는 함수 (총 4번 호출)
def rotate(key):
    arr = []
    for i in range(len(key)-1, -1, -1):
        tmp = []
        for j in range(len(key)):
            tmp.append(key[j][i])
        arr.append(tmp)
    return arr

# 왼쪽(-)/오른쪽(+)으로 a, 위(-)/아래(+)로 b만큼 이동한 key와 lock을 맞춰 보는 함수
def challenge(key, lock, a, b):
		# 2차원 배열을 사용하므로, list 안의 list는 call by reference. 
		# 가령, arr[0]을 수정하면 lock[0]이 수정됨. 이를 해소하기 위해 deepcopy 사용
    arr = deepcopy(lock)
    for i in range(len(arr)):
        if i+a < 0: continue
        if i+a > len(key)-1: break
        for j in range(len(arr)):
            if j+b < 0: continue
            if j+b > len(key) - 1: break
            arr[i][j] += key[i+a][j+b]
    # 최종적으로 모든 원소가 1일 때 합격
    return sum(arr, []) == [1 for _ in range(len(arr)**2)]

def solution(key, lock):
    for _ in range(4):
        # 반복문의 범위는 key를 왼쪽/위, 오른쪽/아래로 움직일 수 있는 최대치
        for i in range(-len(lock)+1, len(lock)):
            for j in range(-len(lock)+1, len(lock)):
                if challenge(key, lock, i, j): return True
        key = rotate(key)
    return False
```