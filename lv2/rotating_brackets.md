# 문제

다음 규칙을 지키는 문자열을 올바른 괄호 문자열이라고 정의합니다.

- `()`, `[]`, `{}` 는 모두 올바른 괄호 문자열입니다.
- 만약 `A`가 올바른 괄호 문자열이라면, `(A)`, `[A]`, `{A}` 도 올바른 괄호 문자열입니다. 예를 들어, `[]` 가 올바른 괄호 문자열이므로, `([])` 도 올바른 괄호 문자열입니다.
- 만약 `A`, `B`가 올바른 괄호 문자열이라면, `AB` 도 올바른 괄호 문자열입니다. 예를 들어, `{}` 와 `([])` 가 올바른 괄호 문자열이므로, `{}([])` 도 올바른 괄호 문자열입니다.

대괄호, 중괄호, 그리고 소괄호로 이루어진 문자열 `s`가 매개변수로 주어집니다. 이 `s`를 왼쪽으로 x (*0 ≤ x < (`s`의 길이)*) 칸만큼 회전시켰을 때 `s`가 올바른 괄호 문자열이 되게 하는 x의 개수를 return 하도록 solution 함수를 완성해주세요.

## 제한사항

- s의 길이는 1 이상 1,000 이하입니다.

## 예제

| s | result |
| "[](){}" | 3 |
| "}]()[{" | 2 |
| "[)(]" | 0 |
| "}}}" | 0 |


# 풀이

python을 이용한 풀이는 아래와 같습니다.

- 스택을 사용합니다.

```python
def is_right_bracket(s):
    stack = []
    for c in s:
				# 왼쪽 괄호가 나왔을 때 스택에 쌓임
        if c in ["[", "(", "{"]:
            stack.append(c)
            continue

				# 짝이 맞는 괄호가 나왔을 때, 스택에 쌓인 왼쪽 괄호가 사라짐        
        if stack != [] and c == "]" and stack[-1] == "[":
            stack.pop()
        elif stack != [] and c == ")" and stack[-1] == "(":
            stack.pop()
        elif stack != [] and c == "}" and stack[-1] == "{":
            stack.pop()
        else:
            pass
    
		# 최종적으로 스택이 비워지면, 올바른 괄호 문자열
    return len(stack) == 0
    
def is_fair_bracket(s):
		# 애초에 괄호의 개수가 맞지 않는 경우 미리 걸러내는 함수
		# 문자열의 길이가 홀수인 경우 성립 불가
    if len(s) % 2 == 1: return False
		# 각 괄호의 짝이 맞지 않는 경우 성립 불가
    if len([c for c in s if c == "{"]) != len([c for c in s if c == "}"]): return False
    if len([c for c in s if c == "("]) != len([c for c in s if c == ")"]): return False
    if len([c for c in s if c == "["]) != len([c for c in s if c == "]"]): return False
    return True
    
def solution(s):
    answer = 0
    if is_fair_bracket(s) == False:
        return 0
    
		# for문을 통해 순회하며 회전할 때 올바른 괄호 문자열인지 체크하고, 올바른 괄호 문자열이면 answer 증가
    for i in range(len(s)):
        if is_right_bracket(s): answer += 1
        s = s[1:]+s[0]
    
    return answer
```