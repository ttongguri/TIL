# BFS(너비 우선 탐색) 완전 가이드

## BFS란?

BFS(Breadth-First Search, 너비 우선 탐색)는 그래프나 트리를 탐색하는 알고리즘이다. 시작점에서 가까운 노드부터 차례대로 탐색하며, 같은 깊이(레벨)에 있는 모든 노드를 먼저 방문한 후 다음 깊이로 이동한다.

## 큐를 이용한 BFS 동작 원리

### 핵심 아이디어
- **큐(Queue)**: FIFO(First In First Out) 구조를 활용
- **레벨별 탐색**: 현재 레벨의 모든 노드를 처리한 후 다음 레벨로 진행
- **방문 체크**: 이미 방문한 노드는 다시 방문하지 않음

### 동작 과정
1. 시작 노드를 큐에 삽입하고 방문 표시
2. 큐가 비어있지 않은 동안 반복:
   - 큐에서 노드를 하나 꺼냄(dequeue)
   - 해당 노드와 인접한 모든 노드를 확인
   - 방문하지 않은 인접 노드를 큐에 삽입하고 방문 표시

## BFS 구현 방법

### 1. 그래프 BFS (인접 리스트)

```python
from collections import deque

def bfs_graph(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        # 인접한 노드들을 확인
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result

# 사용 예시
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

print(bfs_graph(graph, 'A'))  # ['A', 'B', 'C', 'D', 'E', 'F']
```

### 2. 2D 격자에서의 BFS

```python
from collections import deque

def bfs_grid(grid, start_row, start_col):
    rows, cols = len(grid), len(grid[0])
    visited = [[False] * cols for _ in range(rows)]
    queue = deque([(start_row, start_col)])
    visited[start_row][start_col] = True
    
    # 상하좌우 이동 방향
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    while queue:
        row, col = queue.popleft()
        print(f"방문: ({row}, {col})")
        
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc
            
            # 범위 체크 및 방문 체크
            if (0 <= new_row < rows and 
                0 <= new_col < cols and 
                not visited[new_row][new_col] and 
                grid[new_row][new_col] == 1):  # 1은 이동 가능한 칸
                
                visited[new_row][new_col] = True
                queue.append((new_row, new_col))

# 사용 예시
grid = [
    [1, 1, 0, 1],
    [1, 1, 1, 0],
    [0, 1, 1, 1],
    [1, 0, 1, 1]
]
bfs_grid(grid, 0, 0)
```

### 3. 최단 거리를 구하는 BFS

```python
from collections import deque

def bfs_shortest_path(graph, start, target):
    if start == target:
        return 0
    
    visited = set([start])
    queue = deque([(start, 0)])  # (노드, 거리)
    
    while queue:
        node, distance = queue.popleft()
        
        for neighbor in graph[node]:
            if neighbor == target:
                return distance + 1
            
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, distance + 1))
    
    return -1  # 경로가 없는 경우

# 최단 경로를 저장하는 BFS
def bfs_path(graph, start, target):
    if start == target:
        return [start]
    
    visited = set([start])
    queue = deque([(start, [start])])  # (노드, 경로)
    
    while queue:
        node, path = queue.popleft()
        
        for neighbor in graph[node]:
            if neighbor == target:
                return path + [neighbor]
            
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    
    return []  # 경로가 없는 경우
```

## BFS vs DFS 비교

| 특성 | BFS (너비 우선 탐색) | DFS (깊이 우선 탐색) |
|------|---------------------|---------------------|
| **자료구조** | 큐(Queue) | 스택(Stack) 또는 재귀 |
| **탐색 순서** | 레벨별로 가까운 노드부터 | 한 방향으로 끝까지 탐색 후 백트래킹 |
| **메모리 사용** | 현재 레벨의 모든 노드 저장 | 현재 경로상의 노드만 저장 |
| **최단 경로** | 가중치가 없는 그래프에서 최단 경로 보장 | 최단 경로 보장하지 않음 |
| **시간 복잡도** | O(V + E) | O(V + E) |
| **공간 복잡도** | O(V) - 최악의 경우 | O(h) - h는 최대 깊이 |

### DFS 구현 예시 (비교용)

```python
# 재귀를 이용한 DFS
def dfs_recursive(graph, node, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(node)
    print(node, end=' ')
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)

# 스택을 이용한 DFS
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            result.append(node)
            # 역순으로 추가 (스택이므로)
            for neighbor in reversed(graph[node]):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return result
```

## 언제 BFS를 사용할까?

### BFS가 적합한 문제들

1. **최단 경로 문제**
   - 가중치가 없는 그래프에서의 최단 거리
   - 미로 탈출의 최단 경로
   - 예: "미로에서 출구까지의 최단 거리는?"

2. **레벨별 탐색이 필요한 문제**
   - 트리의 레벨 순회
   - 같은 깊이의 노드들을 처리해야 하는 경우
   - 예: "각 레벨별 노드의 합을 구하라"

3. **연결 요소 찾기**
   - 섬의 개수 세기
   - 연결된 구성요소 찾기
   - 예: "연결된 육지의 크기는?"

4. **최소 스텝 문제**
   - 상태 공간에서 목표 상태까지의 최소 변환 횟수
   - 예: "단어 변환", "물통 문제"

### BFS 문제 예시

```python
# 예시 1: 미로에서 최단 경로
def shortest_path_maze(maze, start, end):
    from collections import deque
    
    rows, cols = len(maze), len(maze[0])
    queue = deque([(start[0], start[1], 0)])  # (row, col, distance)
    visited = set([start])
    directions = [(-1,0), (1,0), (0,-1), (0,1)]
    
    while queue:
        row, col, dist = queue.popleft()
        
        if (row, col) == end:
            return dist
        
        for dr, dc in directions:
            nr, nc = row + dr, col + dc
            if (0 <= nr < rows and 0 <= nc < cols and 
                (nr, nc) not in visited and maze[nr][nc] == 0):
                visited.add((nr, nc))
                queue.append((nr, nc, dist + 1))
    
    return -1

# 예시 2: 이진 트리의 레벨별 합
def level_sum(root):
    if not root:
        return []
    
    from collections import deque
    queue = deque([root])
    level_sums = []
    
    while queue:
        level_size = len(queue)
        level_sum = 0
        
        for _ in range(level_size):
            node = queue.popleft()
            level_sum += node.val
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        level_sums.append(level_sum)
    
    return level_sums
```

### DFS가 더 적합한 문제들

1. **경로 탐색 문제**
   - 모든 가능한 경로 찾기
   - 백트래킹이 필요한 문제
   - 예: N-Queen, 순열/조합 생성

2. **깊이 제한이 있는 탐색**
   - 메모리 사용량을 줄여야 하는 경우
   - 무한히 깊은 그래프에서의 탐색

3. **위상 정렬**
   - 사이클 검출
   - 의존성 관계 파악

## 시간/공간 복잡도 분석

### 시간 복잡도: O(V + E)
- V: 정점(노드)의 개수
- E: 간선의 개수
- 각 정점과 간선을 한 번씩만 방문

### 공간 복잡도: O(V)
- 방문 배열: O(V)
- 큐의 크기: 최악의 경우 O(V)
- 특히 완전 이진 트리에서는 마지막 레벨에 V/2개의 노드가 큐에 저장될 수 있음

## 주의사항 및 팁

1. **방문 체크 시점**: 큐에 삽입할 때 방문 표시를 해야 중복 방문을 방지할 수 있다.

2. **메모리 최적화**: 큰 그래프에서는 메모리 사용량에 주의해야 한다.

3. **입력 검증**: 시작점이 유효한지, 그래프가 올바르게 구성되었는지 확인한다.

4. **방향 설정**: 2D 격자에서는 대각선 이동 여부를 명확히 해야 한다.

```python
# 8방향 (대각선 포함)
directions_8 = [(-1,-1), (-1,0), (-1,1), (0,-1), (0,1), (1,-1), (1,0), (1,1)]

# 4방향 (상하좌우만)
directions_4 = [(-1,0), (1,0), (0,-1), (0,1)]
```

BFS는 레벨별 탐색과 최단 거리 문제에서 매우 유용한 알고리즘이다. 큐의 FIFO 특성을 잘 이해하고 활용하면 다양한 문제를 효율적으로 해결할 수 있다.