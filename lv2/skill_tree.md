# 스킬 트리

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/49993)

[돌아가기](/../alg/)

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더`일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 `스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크`나 `라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리(유저가 스킬을 배울 순서)를 담은 배열 `skill_trees`가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

## 제한 사항

- 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 스킬 순서와 스킬트리는 문자열로 표기합니다.
    - 예를 들어, `C → B → D` 라면 "CBD"로 표기합니다
- 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
- skill_trees는 길이 1 이상 20 이하인 배열입니다.
- skill_trees의 원소는 스킬을 나타내는 문자열입니다.
    - skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

## 예제

| skill | skill_trees | return |
| --- | --- | --- |
| "CBD" | ["BACDE", "CBADF", "AECB", "BDA"] | 2 |

- "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.
- "CBADF": 가능한 스킬트리입니다.
- "AECB": 가능한 스킬트리입니다.
- "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 문자열 조작을 사용합니다.
    - 각 skill_tree에서 스킬 순서가 설정되지 않은 문자열을 제거합니다.
    - 제거한 뒤, skill에 지정된 순서대로 스킬을 익히고 있는지 확인합니다.
    - 이렇게 확인하여 일치한 개수를 고릅니다.

```python
def skillcheck(skill, skill_tree):
    arr = [e for e in list(skill_tree) if e in list(skill)]
    
    return arr == list(skill)[:len(arr)]
    
def solution(skill, skill_trees):
    return len([skill_tree for skill_tree in skill_trees if skillcheck(skill, skill_tree)])
```