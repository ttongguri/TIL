# 깊이 우선 탐색(DFS)과 동적 계획법(DP) 완전 정리

## 1. 깊이 우선 탐색 (Depth-First Search, DFS)

### 개념 정리
깊이 우선 탐색은 그래프나 트리를 탐색하는 알고리즘으로, **한 경로를 끝까지 탐색한 후 다음 경로를 탐색**하는 방식입니다.

**핵심 특징:**
- 스택(Stack) 또는 재귀함수를 사용
- 메모리 사용량이 상대적으로 적음
- 깊은 곳까지 빠르게 탐색 가능
- 최단 경로를 보장하지 않음

### DFS 동작 원리
1. 시작 노드를 스택에 넣고 방문 처리
2. 스택에서 노드를 꺼내고 인접한 노드 중 방문하지 않은 노드를 스택에 넣음
3. 스택이 빌 때까지 2번 과정 반복

### 예시 코드

#### 1) 재귀 함수를 이용한 DFS
```python
def dfs_recursive(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    print(start, end=' ')
    
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
    
    return visited

# 그래프 예시 (인접 리스트)
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

print("DFS 탐색 결과:")
dfs_recursive(graph, 'A')
```

#### 2) 스택을 이용한 DFS
```python
def dfs_stack(graph, start):
    visited = set()
    stack = [start]
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            print(node, end=' ')
            
            # 역순으로 스택에 추가 (왼쪽부터 탐색하기 위해)
            for neighbor in reversed(graph[node]):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return visited

print("\n스택 DFS 탐색 결과:")
dfs_stack(graph, 'A')
```

#### 3) 2차원 배열에서의 DFS (미로 탐색)
```python
def dfs_maze(maze, start_x, start_y, end_x, end_y):
    rows, cols = len(maze), len(maze[0])
    visited = [[False] * cols for _ in range(rows)]
    
    # 상, 하, 좌, 우 방향
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    def dfs(x, y):
        if x == end_x and y == end_y:
            return True
        
        visited[x][y] = True
        
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            
            if (0 <= nx < rows and 0 <= ny < cols and 
                not visited[nx][ny] and maze[nx][ny] == 0):
                if dfs(nx, ny):
                    return True
        
        return False
    
    return dfs(start_x, start_y)

# 미로 예시 (0: 길, 1: 벽)
maze = [
    [0, 1, 0, 0, 0],
    [0, 1, 0, 1, 0],
    [0, 0, 0, 1, 0],
    [1, 1, 0, 0, 0],
    [0, 0, 0, 1, 0]
]

result = dfs_maze(maze, 0, 0, 4, 4)
print(f"\n미로 탈출 가능: {result}")
```

### DFS 활용 예시
1. **경로 찾기 문제**: 미로 탈출, 그래프에서 두 노드 간 경로 존재 확인
2. **연결 요소 찾기**: 그래프의 연결된 컴포넌트 개수 구하기
3. **사이클 검출**: 그래프에서 사이클 존재 여부 확인
4. **위상 정렬**: DAG(Directed Acyclic Graph)에서 위상 정렬

---

## 2. 동적 계획법 (Dynamic Programming, DP)

### 개념 정리
동적 계획법은 **복잡한 문제를 간단한 하위 문제들로 나누어 해결**하는 알고리즘 기법입니다.

**핵심 특징:**
- 중복되는 하위 문제들의 해답을 저장하여 재사용
- 최적 부분 구조를 가져야 함
- 메모이제이션(Memoization) 또는 테이블 방식 사용

### DP가 적용되는 조건
1. **최적 부분 구조**: 큰 문제의 최적해가 작은 문제들의 최적해로 구성
2. **중복되는 부분 문제**: 같은 하위 문제가 여러 번 계산됨

### 예시 코드

#### 1) 피보나치 수열 - 기본 재귀 vs DP
```python
# 기본 재귀 (비효율적)
def fibonacci_recursive(n):
    if n <= 1:
        return n
    return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)

# DP - 메모이제이션 방식
def fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    return memo[n]

# DP - 테이블 방식 (Bottom-up)
def fibonacci_table(n):
    if n <= 1:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

# 성능 비교
import time

n = 35
start = time.time()
result1 = fibonacci_recursive(n)
time1 = time.time() - start

start = time.time()
result2 = fibonacci_memo(n)
time2 = time.time() - start

start = time.time()
result3 = fibonacci_table(n)
time3 = time.time() - start

print(f"재귀: {result1}, 시간: {time1:.4f}초")
print(f"메모: {result2}, 시간: {time2:.4f}초")
print(f"테이블: {result3}, 시간: {time3:.4f}초")
```

#### 2) 0-1 배낭 문제
```python
def knapsack_01(weights, values, capacity):
    n = len(weights)
    # dp[i][w] = i번째까지 아이템을 고려했을 때, 용량 w에서의 최대 가치
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            # i번째 아이템을 포함하지 않는 경우
            dp[i][w] = dp[i-1][w]
            
            # i번째 아이템을 포함하는 경우
            if weights[i-1] <= w:
                include_value = dp[i-1][w - weights[i-1]] + values[i-1]
                dp[i][w] = max(dp[i][w], include_value)
    
    return dp[n][capacity]

# 선택된 아이템들을 추적하는 함수
def knapsack_with_items(weights, values, capacity):
    n = len(weights)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    # DP 테이블 채우기
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            dp[i][w] = dp[i-1][w]
            if weights[i-1] <= w:
                include_value = dp[i-1][w - weights[i-1]] + values[i-1]
                dp[i][w] = max(dp[i][w], include_value)
    
    # 선택된 아이템 추적
    selected_items = []
    w = capacity
    for i in range(n, 0, -1):
        if dp[i][w] != dp[i-1][w]:
            selected_items.append(i-1)
            w -= weights[i-1]
    
    return dp[n][capacity], selected_items[::-1]

# 예시
weights = [2, 3, 4, 5]
values = [3, 4, 5, 6]
capacity = 8

max_value, items = knapsack_with_items(weights, values, capacity)
print(f"최대 가치: {max_value}")
print(f"선택된 아이템 인덱스: {items}")
```

#### 3) 최장 공통 부분 수열 (LCS)
```python
def lcs_length(text1, text2):
    m, n = len(text1), len(text2)
    # dp[i][j] = text1[:i]와 text2[:j]의 LCS 길이
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

def lcs_string(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # DP 테이블 채우기
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    # LCS 문자열 구성
    lcs = []
    i, j = m, n
    while i > 0 and j > 0:
        if text1[i-1] == text2[j-1]:
            lcs.append(text1[i-1])
            i -= 1
            j -= 1
        elif dp[i-1][j] > dp[i][j-1]:
            i -= 1
        else:
            j -= 1
    
    return ''.join(reversed(lcs))

# 예시
text1 = "ABCDGH"
text2 = "AEDFHR"
print(f"LCS 길이: {lcs_length(text1, text2)}")
print(f"LCS 문자열: {lcs_string(text1, text2)}")
```

#### 4) 계단 오르기 문제
```python
def climb_stairs(n):
    """
    n개의 계단이 있고, 1개 또는 2개씩 오를 수 있을 때
    꼭대기에 도달하는 방법의 수
    """
    if n <= 2:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    dp[2] = 2
    
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

# 공간 최적화 버전
def climb_stairs_optimized(n):
    if n <= 2:
        return n
    
    prev2 = 1  # dp[i-2]
    prev1 = 2  # dp[i-1]
    
    for i in range(3, n + 1):
        current = prev1 + prev2
        prev2 = prev1
        prev1 = current
    
    return prev1

print(f"5개 계단 오르는 방법: {climb_stairs(5)}가지")
```

### DP 활용 예시

#### 1. 최적화 문제
- 배낭 문제, 동전 교환 문제, 최단 경로 문제

#### 2. 조합론 문제
- 경우의 수 계산, 파스칼 삼각형

#### 3. 문자열 처리
- 편집 거리, 최장 공통 부분 수열, 팰린드롬

#### 4. 그래프 문제
- 플로이드-워셜 알고리즘, 벨만-포드 알고리즘

---

## 3. DFS와 DP의 차이점과 연관성

### 주요 차이점

| 구분 | DFS | DP |
|------|-----|-----|
| **목적** | 그래프/트리 탐색 | 최적해 찾기 |
| **접근법** | 깊이 우선 탐색 | 부분 문제 해결 |
| **메모리** | 스택/재귀 | 테이블/메모 |
| **시간복잡도** | O(V+E) | 문제에 따라 다름 |
| **특징** | 경로 탐색 | 중복 계산 방지 |

### 연관성과 결합 사례

DFS와 DP는 함께 사용되는 경우가 많습니다:

```python
# 트리에서 각 노드를 루트로 하는 서브트리의 크기 구하기
def subtree_size_dfs_dp(graph, root):
    memo = {}  # DP 메모이제이션
    
    def dfs(node, parent):
        if node in memo:
            return memo[node]
        
        size = 1  # 자기 자신
        for neighbor in graph[node]:
            if neighbor != parent:
                size += dfs(neighbor, node)
        
        memo[node] = size
        return size
    
    return dfs(root, -1)

# 트리 그래프 예시
tree = {
    1: [2, 3],
    2: [1, 4, 5],
    3: [1, 6],
    4: [2],
    5: [2],
    6: [3]
}

print(f"루트 1의 서브트리 크기: {subtree_size_dfs_dp(tree, 1)}")
```

## 4. 학습 팁과 연습 문제

### 학습 팁
1. **DFS**: 재귀 구조를 이해하고, 스택의 동작 원리를 파악하세요
2. **DP**: 작은 문제부터 시작해서 점진적으로 큰 문제로 확장하는 사고방식을 기르세요
3. **실습**: 다양한 문제를 직접 구현해보며 패턴을 익히세요

### 추천 연습 문제
- **DFS**: 섬의 개수, 단지번호붙이기, 타겟 넘버
- **DP**: 1로 만들기, 정수 삼각형, 가장 긴 증가하는 부분 수열

이 자료를 바탕으로 차근차근 연습하시면 두 알고리즘을 모두 마스터할 수 있을 것입니다!