# 체육복

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

## 제한사항

- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

## 예제

| n | lost | reserve | return |
| - | - | - | - |
| 5 | [2,4] | [1,3,5] | 5 |
| 5 | [2,4] | [3] | 4 |
| 3 | [3] | [1] | 2 |

예제 #1

1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

예제 #2

3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 완전 탐색, 탐욕법을 이용합니다(앞 번호 우선).
    - 여벌이 있는 학생 중 자기 체육복을 잃어버린 학생은 아예 탐색에서 제외합니다.

```python
def solution(n, lost2, reserve2):
		# lost, reserve에 모두 해당되는 경우는 처음부터 없는 것으로 만들고 시작
    lost = [student for student in lost2 if student not in reserve2]
    reserve = [student for student in reserve2 if student not in lost2]
    
		# 정렬
    lost.sort()
    reserve.sort()
    
		# 기본적으로 전체 학생 - 잃어버린 학생에서 시작
    answer = n - len(lost)
    
		# 잃어버린 학생 중 양 옆 학생에게 여벌을 빌릴 수 있는 경우
		# 앞 번호 우선으로 빌리도록 함
    for student in lost:
        candidate1 = student-1
        candidate2 = student+1
        if candidate1 in reserve:
            reserve.remove(candidate1)
            answer += 1
        elif candidate2 in reserve:
            reserve.remove(candidate2)
            answer += 1
        else:
            pass
        
    return answer
```