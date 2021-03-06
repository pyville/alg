## 올바른 괄호

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/12909)

[돌아가기](/../alg/)

괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어

- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 `true`를 return 하고, 올바르지 않은 괄호이면 `false`를 return 하는 solution 함수를 완성해 주세요.

## 제한 사항

- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

## 예제

| s | answer |
| - | - |
| "()()" | True |
| "(())()" | True |
| ")()(" | False |
| "(()(" | False |

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 스택을 사용합니다.
    - 여는 괄호는 스택에 저장합니다.
    - 닫는 괄호는 스택에 저장하지 않고, 대신 스택에 저장된 여는 괄호를 하나 꺼냅니다.

```python
def solution(s):
    stack = []
    for c in s:
        if stack != [] and stack[-1] == '(' and c == ')':
            stack.pop()
            continue
        stack.append(c)
        
    return stack == []
```