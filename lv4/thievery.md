# 도둑질

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/42897)

[돌아가기](/../alg/)

> [스티커 모으기(2)](https://programmers.co.kr/learn/courses/30/lessons/12971) 문제 또한 사실상 동일한 방식으로 풀 수 있습니다. 둘 다 도전해 보세요!

도둑이 어느 마을을 털 계획을 하고 있습니다. 이 마을의 모든 집들은 아래 그림과 같이 동그랗게 배치되어 있습니다.

![examples](https://user-images.githubusercontent.com/52960121/119263596-a8acca80-bc1a-11eb-85b4-4fc9568d34b4.png)

각 집들은 서로 인접한 집들과 방범장치가 연결되어 있기 때문에 인접한 두 집을 털면 경보가 울립니다.

각 집에 있는 돈이 담긴 배열 money가 주어질 때, 도둑이 훔칠 수 있는 돈의 최댓값을 return 하도록 solution 함수를 작성하세요.

## 제한사항

- 이 마을에 있는 집은 3개 이상 1,000,000개 이하입니다.
- money 배열의 각 원소는 0 이상 1,000 이하인 정수입니다.

## 예제

| money | return |
| - | - |
| [1,2,3,1] | 4 |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 동적 계획법을 사용합니다.
    - 일직선으로 된 집들을 터는 문제는 간단하게 (i번째 집까지 털 때의 최댓값) = (i-1번째 집까지 털 때의 최댓값), (i-2번째 집까지 털 때의 최댓값 + i번째 집을 턴 값) 중 더 큰 값이 성립합니다.
    - 동그랗게 배치되어 있는 경우는 첫 집과 마지막 집 사이의 고리를 인위적으로 끊고, 첫 집을 털 때(마지막 집 터는 것이 불가능)와 첫 집을 털지 않을 때(마지막 집 터는 것도 가능) 각각의 경우를 계산해서 일직선인 것처럼 수행합니다.

```python
def solution(money):
    answer = 0
    # 첫번째 집을 털음 (마지막 집을 털 수 없음)
    arr1 = [money[0], money[0]]
    # 첫번째 집을 안 털음 (마지막 집을 털 수 있음)
    arr2 = [0, money[1]]
    
    for i in range(2, len(money)-1):
        arr1.append(max(arr1[i-1], arr1[i-2]+money[i]))
        
    for i in range(2, len(money)):
        arr2.append(max(arr2[i-1], arr2[i-2]+money[i]))
    
    return max(arr1.pop(), arr2.pop())
```
