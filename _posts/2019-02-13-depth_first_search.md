---
title: 깊이 우선 탐색 *
tags:
  - 깊이우선탐색
  - Depth First Search

categories:
  - Algorithm
---

- 제목에 * 표시가 있는 것은 추가할 내용이 있거나 수정할 내용이 있다는 표시입니다.

# 깊이우선 탐색(DFS)

어떤 데이터를 탐색해야할 때 흔히 for와 while 반복문을 사용합니다. 정갈하게 순서대로 나열되어 있는 자료의 경우 이 두가지를 사용해도 충분히 전체 탐색을 할 수가 있습니다. 하지만 그렇지 않은 경우에는 어떻게 해야 할까요? 예를 들어 두사람이 오목을 둘때 먼저 둔 사람이 이길수 있는 경우의 수를 뽑아내야 한다면 for와 while 문 만으로 해결하기엔 벅찰 것입니다. 이럴 때 사용하는 것이 **깊이우선 탐색(Depth First Search)** 과 **너비우선 탐색(Breadth First Search)** 입니다. 여기서는 깊이우선 탐색에 대해 알아보겠습니다. 설명에 앞서 그래프에 대해서 먼저 이해하고 가겠습니다.

위에서 오목에 대한 예시를 들었으니 이번에도 오목으로 예를 들겠습니다. 오목을 처음 시작했을 때 오목판에 바둑돌이 하나도 놓이지 않은 상태를 노드로 만듭니다. 그리고 먼저 바둑돌을 두는 사람이 놓을 수 있는 모든 방법을 엣지로 만듭니다. 각 엣지의 경우대로 바둑돌을 놓았을때 발생하는 돌의 배치를 새로운 노드로 만듭니다. 그리고 상대방이 그에 대응해 바둑돌을 놓을 수 있는 방법을 엣지로 표현합니다. 이를 반복하면 바둑돌의 배치를 그래프의 노드로, 바둑돌을 놓을 수 있는 방법을 엣지로 하는 그래프를 만들 수 있게 됩니다. (아래 그림은 예시와 관련없는 그래프의 그림입니다.)

<center><img data-action="zoom" src='{{ "/assets/img/graph_01.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://www.thesearchagency.com/2017/01/supercharge-influencer-marketing-efforts-social-graph-theory/">출처</a></center>
<br/>

이제 이렇게 표현된 그래프로 **깊이우선 탐색** 에 대해 자세히 알아보겠습니다.

<center><img data-action="zoom" src='{{ "/assets/img/dfs_01.png" | relative_url }}' alt='absolute'></center>
<center><a href="https://www.researchgate.net/figure/Fig-4-Illustrating-DFS-algorithm_fig2_271523961">출처</a></center>
<br/>

깊이우선 탐색은 이름에서도 직관적으로 알수 있듯이 그래프의 아래방향(깊이)를 우선적으로 탐색하는 방법입니다. 위 그림으로 차근차근 설명해 보겠습니다.

탐색의 시작은 노드 A부터 시작합니다. 그리고 A에서 아래방향으로 갈 수 있는 곳이 있나 찾습니다. 일단 B와 C가 있지만 왼쪽에 위치한 것부터 발견하므로 노드 B를 먼저 찾게 됩니다. 그래서 그림에서 처럼 **1** 번 화살표 이동을 하게됩니다. 이렇게 깊이우선 탐색 규칙에 따라서 노드 B 다음에 위치한 노드를 탐색해야합니다. 하지만 노드 B를 조사하고 난 후에는 노드 A로 다시 돌아와야 하는데 B 노드 이전에 A노드를 거쳤다는 것을 어떻게 알 수 있을까요? 그리고 노드 A로 돌아왔을 때, 노드 B에 방문했었는지는 어떻게 알수 있을까요?

 노드 B를 탐색하기 이전에 노드 A에 방문했다고 표시해놓으면 이후에 돌아와서도 탐색을 계속할 수 있습니다. 그러므로 노드 A를 스택에 넣습니다.

 > 스택: A | | | | ...

스택에 쌓여있던 노드 A를 꺼내어 출력하고 노드 A의 자식노드인 노드 B와 노드 C를 다시 스택에 쌓아줍니다.

 > 스택: C | B | | | ...
 출력: A

 마찬가지로 마지막에 쌓여있는 노드 B를 꺼내어 출력하고 자식노드인 노드 D를 쌓아줍니다.

 > C | D | | | ...
 출력: A B

 다시 노드 D를 꺼내어 출력하고 노드 D의 자식노드인 노드 G와 노드 H를 스택에 쌓아줍니다.

 > C | H | G | | ...
 출력: A B D

 스택 맨위에 쌓여있는 노드 G를 꺼내어 출력하고 노드 G의 자식노드를 스택에 쌓아야하는데 노드 G에는 자식노드가 없으므로 쌓을 노드가 없습니다.

 > C | H | | | ...
 출력: A B D G

 다시 스택 맨위에 쌓여있는 노드 H를 꺼내어 출력하고 노드 H의 자식노드를 쌓아야하지만 역시 자식노드가 없으므로 쌓아야할 노드 또한 없습니다.

 > C | | | | ...
 출력: A B D G H

 이제 스택에 남아있는 노드는 노드 C 하나입니다. 노드 C를 꺼내어 출력하고 자식노드인 노드 E와 노드 F를 스택에 쌓아줍니다.

 > F | E | | | ...
 출력: A B D G H C

 쭉 이러한 과정을 반복해나가며 스택에서 노드를 꺼내어 출력하고 꺼낸 노드의 자식노드를 쌓아올리고를 반복하여 줍니다. 스택에 노드가 남아있지 않을 때까지 과정을 반복하면 출력결과물(방문노드 순서)은 다음과 같습니다.

 > 출력: A B D G H C E I J F K L

 이 과정을 파이썬 코드로 구현해 보겠습니다. graph는 <a href="https://en.wikipedia.org/wiki/Adjacency_list">Adjacency list(인접 리스트)</a> 로 표현하였습니다.

```python
graph = {'A': set(['B', 'C']),
         'B': set(['D']),
         'C': set(['E', 'F']),
         'D': set(['G', 'H']),
         'E': set(['I', 'J']),
         'F': set(['K', 'L']),
         'G': set(['D']),
         'H': set(['D']),
         'I': set(['E']),
         'J': set(['E']),
         'K': set(['F']),
         'L': set(['F'])}

def dfs(graph, start):
    visited = []
    stack = [start]

    while stack:
        node = stack.pop()

        if node not in visited:

            visited.append(node)
            childnode = sorted(graph[node] - set(visited))
            childnode.reverse()

            stack += childnode

    return visited

dfs(graph, 'A')
```

위 `def` function 에서 아래 코드에 해당하는 부분은 자식노드중 맨 왼쪽 노드를 우선 탐색하기 위한 코드입니다.  

```python
childnode = sorted(graph[node] - set(visited))
childnode.reverse()
```

이로써 깊이우선 탐색에 대한 설명을 마치겠습니다. **너비우선 탐색(Breadth First Search)** 설명에 앞서서 재미있는 그림하나를 보고 이해를 도운 뒤 넘어가겠습니다.

<br/>
<center><a href="http://ai.berkeley.edu/home.html">CS188x강의</a></center>
<center><img data-action="zoom" src='{{ "/assets/img/dfs_02.png" | relative_url }}' alt='absolute'></center>
<center>깊이우선 탐색</center>
<center><img data-action="zoom" src='{{ "/assets/img/dfs_03.png" | relative_url }}' alt='absolute'></center>
<center>너비우선 탐색</center>
<br/>
