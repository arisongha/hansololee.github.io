---
title: 너비 우선 탐색(BFS) *
tags:
  - 너비우선탐색
  - Breadth First Search

categories:
  - AlgorithmTest
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.
- 이 글은 책 <a href="http://www.hanbit.co.kr/store/books/look.php?p_code=B9653641350">TopCoder 알고리즘 트레이닝</a>을 참고하였음을 미리 밝힙니다.

# 너비우선 탐색(BFS)

너비우선 탐색은 보통 앞서 설명한 깊이우선 탐색과 같이 설명하는 전체탐색 개념 중 하나입니다. 깊이우선 탐색이 한 노드의 아래방향으로 획을 따라 쭉 내려가면서 탐색을 했다면 너비우선 탐색은 가까운 순서대로 차근차근 탐새을 합니다. 깊이우선 탐색과 다른 점을 2가지로 표현하자면 아래와 같습니다.

- 이전 정점으로 돌아올 필요가 없다
- 가까운 순서대로 탐색하므로 나중에 발견한 정점은 나중에 탐색한다.

깊이우선 탐색이 자료구조 스택을 사용하여 메모리에 쌓인 노드 중 맨 마지막에 쌓인 것부터 꺼내어 탐색했다면, 너비우선 탐색은 그와는 반대로 큐를 사용하여 메모리에 추가된 노드 중 가장 오래된 것부터 차례대로 탐색해야 합니다. 그럼 탐색 과정을 아래 그래프를 보면서 이해하겠습니다.

<br/>
<center><img data-action="zoom" src='{{ "/assets/img/bfs_01.jpg" | relative_url }}' alt='absolute'></center><center><a href="https://www.hackerearth.com/practice/algorithms/graphs/breadth-first-search/tutorial/">출처</a>
<br/>

우선 제일 상단에 있는 노드 0를 먼저 탐색합니다.

> 큐: 0 | | | | ...
출력:

큐에 들어가있는 노드 0을 꺼내어 출력하고 노드 0의 자식노드인 노드 1, 노드 2, 노드 3을 차례로 추가해줍니다.

> 큐: 1 | 2 | 3 | | ...
출력: 0

그 다음 노드 1을 꺼내어 출력하고 노드 1의 자식 노드인 노드 4, 노드 5를 큐에 넣어줍니다.

> 큐: 2 | 3 | 4 | 5 | ...
출력: 0 1

마찬가지로 노드 2를 꺼내어 출력해주고 노드 2의 자식노드인 노드 6, 노드 7을 큐에 추가해줍니다.

> 큐: 3 | 4 | 5 | 6 | 7 | ...
출력: 0 1 2

노드 3을 꺼내어 출력해주고 노드 3의 자식노드인 노드 7을 추가해줘야 하지만 이미 노드 7이 큐에 들어가 있으므로 넣어주지 않습니다.

> 큐: 4 | 5 | 6 | 7 | ...
출력: 0 1 2 3

역시 노드 4를 꺼내어 출력해주고 노드 4의 자식노드를 추가해주면 되지만 자식노드가 없으므로 추가해주지 않습니다.

> 큐: 5 | 6 | 7 | |...
출력: 0 1 2 3 4

노드 5, 노드 6, 노드 7도 노드 4와 마찬가지로 자식노드가 없으므로 큐에 더이상 추가해줄 노드 없이 출력만 해주게 됩니다. 결과적으로 출력은 아래와 같습니다.

> 출력 : 0 1 2 3 4 5 6 7

이 과정을 파이썬 코드로 구현해 보겠습니다. graph는 깊이우선 탐색 때와 마찬가지로 인접리스트로 표현하였습니다.

```python
graph = {'0': set(['1', '2', '3']),
         '1': set(['4','5']),
         '2': set(['6', '7']),
         '3': set(['7']),
         '4': set(['1']),
         '5': set(['1']),
         '6': set(['2']),
         '7': set(['2','3'])}

 def bfs(graph, start):
     visited = []
     queue = [start]

     while queue:
         node = queue.pop(0)
         if node not in visited:
             visited.append(node)
             childnode = sorted(graph[node] - set(visited))
             queue += childnode
     return visited

bfs(graph, '0')
```
위 `bfs` function 에서 아래 코드에 해당하는 부분은 같은 Layer에 해당하는 노드 중 맨 왼쪽부터 순서대로 탐색하기 위한 코드입니다.

```python
childnode = sorted(graph[node] - set(visited))
```
