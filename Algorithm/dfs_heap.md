# 그래프 DFS 탐색과 힙 자료구조 TIL

### 1. 그래프 DFS (Depth-First Search) 탐색

#### DFS란?
- **깊이 우선 탐색**으로, 그래프나 트리에서 한 방향으로 최대한 깊이 탐색한 후 되돌아가며 다른 경로를 탐색하는 알고리즘
- 스택(Stack) 자료구조를 이용하거나 재귀 함수로 구현

#### DFS의 특징
- 시간 복잡도: O(V + E) (V: 정점 수, E: 간선 수)
- 공간 복잡도: O(V) (재귀 호출 스택)
- 모든 노드를 방문하는 완전 탐색에 적합
- 경로의 특성을 저장해야 하는 문제에 유리

#### DFS 구현 방식
```python
# 재귀 방식
def dfs_recursive(graph, node, visited):
    visited[node] = True
    print(node)
    
    for neighbor in graph[node]:
        if not visited[neighbor]:
            dfs_recursive(graph, neighbor, visited)

# 스택 방식
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            print(node)
            
            for neighbor in graph[node]:
                if neighbor not in visited:
                    stack.append(neighbor)
```

#### DFS 활용 사례
- 연결 요소 찾기
- 사이클 탐지
- 위상 정렬
- 미로 찾기
- 백트래킹 문제

### 2. 힙 (Heap) 자료구조

#### 힙이란?
- **완전 이진 트리** 기반의 자료구조
- 부모 노드와 자식 노드 간의 대소관계가 일정한 규칙을 만족
- 우선순위 큐를 구현하는 가장 효율적인 방법

#### 힙의 종류
- **최대 힙 (Max Heap)**: 부모 노드가 자식 노드보다 큰 값
- **최소 힙 (Min Heap)**: 부모 노드가 자식 노드보다 작은 값

#### 힙의 주요 연산
- **삽입 (Insert)**: O(log n)
  - 마지막 위치에 삽입 후 부모와 비교하며 위치 조정 (힙화)
- **삭제 (Delete)**: O(log n)
  - 루트 노드 제거 후 마지막 노드를 루트로 이동, 자식과 비교하며 위치 조정
- **탐색**: 최댓값/최솟값 O(1)

#### 힙 구현 (배열 기반)
```python
import heapq

# 최소 힙
min_heap = []
heapq.heappush(min_heap, 3)
heapq.heappush(min_heap, 1)
heapq.heappush(min_heap, 4)

print(heapq.heappop(min_heap))  # 1

# 최대 힙 (음수로 변환하여 구현)
max_heap = []
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -1)
heapq.heappush(max_heap, -4)

print(-heapq.heappop(max_heap))  # 4
```

#### 힙 활용 사례
- 우선순위 큐
- 다익스트라 알고리즘
- 힙 정렬
- 상위 K개 원소 찾기
- 중앙값 찾기

## 🔗 DFS와 힙의 연관성

DFS와 힙은 서로 다른 용도의 자료구조이지만, 그래프 알고리즘에서 함께 사용되는 경우가 많습니다:

- **다익스트라 알고리즘**: DFS 기반의 그래프 탐색에 우선순위 큐(힙)를 결합
- **A* 알고리즘**: 휴리스틱 기반 경로 탐색에서 힙으로 우선순위 관리
- **최소 신장 트리**: 프림 알고리즘에서 DFS와 힙을 함께 활용

## 💡 핵심 포인트

### DFS
- 재귀나 스택으로 구현 가능
- 메모리 사용량에 주의 (깊이가 깊을 경우)
- 백트래킹 문제 해결에 적합

### 힙
- 완전 이진 트리의 성질 활용
- 배열로 구현 시 인덱스 관계: 부모(i/2), 왼쪽 자식(2i), 오른쪽 자식(2i+1)
- Python의 heapq는 최소 힙만 지원

## 🎯 다음 학습 계획

- [ ] BFS와 DFS 비교 분석
- [ ] 다익스트라 알고리즘 구현
- [ ] 힙 정렬 구현 및 시간복잡도 분석
- [ ] 백트래킹 문제 해결 실습

---