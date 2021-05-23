# 가장 먼 노드

[문제 풀어보기 (프로그래머스)](https://programmers.co.kr/learn/courses/30/lessons/49189)

[돌아가기](/../alg/)

n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

## 제한 사항

- 노드의 개수 n은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

## 예제

| n | vertex | return |
| - | - | - |
| 6 | [[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]] | 3 |

예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

# 풀이

python을 이용한 풀이는 아래와 같습니다.

- BFS(넓이 우선 탐색)을 사용합니다.
    - g는 {특정 노드: 인접 노드} 형태의 딕셔너리입니다.
    - q는 넓이 우선 탐색을 시행할 리스트(큐)입니다.
    - searched는 이미 탐색한 노드를 기록하여 중복 탐색하지 않게 합니다.
    - depths는 1번 노드와 각 노드 간의 거리를 기록하는 리스트입니다(순서 상관 없음).
    - 정확성 테스트 7, 8, 9를 통과하기 위해서는 searched를 list가 아닌 set으로 사용해야 합니다.

```python
def solution(n, edge):
    g = dict()
		# g에 내용 추가
    for each in edge:
        if each[0] in g:
            g[each[0]] += [each[1]]
        else:
            g[each[0]] = [each[1]]
        if each[1] in g:
            g[each[1]] += [each[0]]
        else:
            g[each[1]] = [each[0]]            
    
		
    q = [(1, 0)]
    searched = set()
    depths = []
    
    while q:
        current_pos, current_dep = q.pop(0)
				# 이미 탐색한 노드를 중복해서 탐색하지 않음
        if current_pos in searched:
            continue
        
				# 현재 노드(pos)와 1번 노드에서부터의 거리(depth)를 기록
        searched.add(current_pos)
        depths.append(current_dep)
        
				# 다음 노드들도 탐색 시작
        for next_pos in g[current_pos]:
            if next_pos not in searched:
                q.append((next_pos, current_dep+1))
    
		# depth의 최대치의 개수를 반환 (몇 번 노드인지는 상관 없음)
    return len([d for d in depths if d == max(depths)])
```