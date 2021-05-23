# 삼각 달팽이

정수 n이 매개변수로 주어집니다. 다음 그림과 같이 밑변의 길이와 높이가 n인 삼각형에서 맨 위 꼭짓점부터 반시계 방향으로 달팽이 채우기를 진행한 후, 첫 행부터 마지막 행까지 모두 순서대로 합친 새로운 배열을 return 하도록 solution 함수를 완성해주세요.

![examples](https://user-images.githubusercontent.com/52960121/119262253-85335100-bc15-11eb-88a5-870dff5d1aa7.png)

## 제한사항

- n은 1 이상 1,000 이하입니다.

## 예제

| n | result |
| --- | --- |
| 4 | [1,2,9,3,10,8,4,5,6,7] |
| 5 | [1,2,12,3,13,11,4,14,15,10,5,6,7,8,9] |
| 6 | [1,2,15,3,16,14,4,17,21,13,5,18,19,20,12,6,7,8,9,10,11] |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 재귀함수를 이용합니다.
    - n=1, n=2, n=3일 때의 경우를 미리 정의해 줍니다.
    - n≥4일 때의 삼각형은 제일 바깥쪽을 1부터 3n-3까지의 값으로 채우고, 3n-2부터 시작하는 n-3의 삼각형이 안에 들어 있는 구조입니다.

```python
def snail(n, start):
    # 초깃값 n=1, n=2, n=3일 때 미리 정의
    if n == 1:
        return [[start + 1]]
    if n == 2:
        return [[start + 1], [start + 2, start + 3]]
    if n == 3:
        return [[start + 1], [start + 2, start + 6], [start + 3, start + 4, start + 5]]
    
    # 삼각형 만들기
    arr = [[i+1+start if j == 0 else 0 for j in range(i+1)] for i in range(n)]

		# 삼각형의 바깥에 접한 칸들만 채우기
    arr[-1] = [k+start for k in range(n, n*2)]
    for i in range(1, n-1):
        arr[i][i] = 3 * n - 2 - i + start
    
    # 삼각형의 안쪽 칸들은 재귀 구조 사용하기
    tmp = snail(n - 3, 3 * n - 3 + start)
    for j in range(2, n-1):
        arr[j][1:-1] = tmp[j-2]
    return arr
    
def solution(n):
		# 2차원 배열을 1차원으로 합치기
    return sum(snail(n, 0), [])
```
