# TIL: 2차원 배열과 델타, 부분집합 알고리즘

## 📋 학습 내용 요약
- 2차원 배열에서 델타를 활용한 이동 처리
- 부분집합 생성과 활용 (비트마스킹, 재귀)
- 실전 문제 해결 패턴

---

## 🎯 2차원 배열과 델타 이동

### 델타 배열의 기본 개념
2차원 배열에서 상하좌우 이동을 구현할 때 델타 배열을 사용하면 코드를 깔끔하게 작성할 수 있습니다.

```python
# 4방향 델타 (상, 하, 좌, 우)
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

# 8방향 델타 (상하좌우 + 대각선)
dx = [-1, -1, -1, 0, 0, 1, 1, 1]
dy = [-1, 0, 1, -1, 1, -1, 0, 1]
```

### 실전 활용 예시
```python
def explore_neighbors(grid, x, y):
    rows, cols = len(grid), len(grid[0])
    neighbors = []
    
    for i in range(4):  # 4방향 탐색
        nx = x + dx[i]
        ny = y + dy[i]
        
        # 경계 체크
        if 0 <= nx < rows and 0 <= ny < cols:
            neighbors.append((nx, ny, grid[nx][ny]))
    
    return neighbors
```

### 미로 탐색 예제
```python
def find_path(maze, start, end):
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]
    
    queue = [start]
    visited = set([start])
    
    while queue:
        x, y = queue.pop(0)
        
        if (x, y) == end:
            return True
            
        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            
            if (0 <= nx < len(maze) and 0 <= ny < len(maze[0]) 
                and maze[nx][ny] == 0 and (nx, ny) not in visited):
                queue.append((nx, ny))
                visited.add((nx, ny))
    
    return False
```

---

## 🔥 부분집합 (Subset) - 핵심 개념

부분집합은 알고리즘 문제에서 매우 자주 등장하는 중요한 개념입니다. 모든 가능한 조합을 고려해야 하는 문제에서 핵심적인 역할을 합니다.

### 1. 비트마스킹을 이용한 부분집합 생성

```python
def generate_subsets_bitmask(arr):
    n = len(arr)
    subsets = []
    
    # 2^n개의 모든 부분집합 생성
    for i in range(1 << n):  # 0부터 2^n-1까지
        subset = []
        for j in range(n):
            if i & (1 << j):  # j번째 비트가 1인지 체크
                subset.append(arr[j])
        subsets.append(subset)
    
    return subsets

# 사용 예시
arr = [1, 2, 3]
result = generate_subsets_bitmask(arr)
print(result)  # [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
```

### 2. 재귀를 이용한 부분집합 생성

```python
def generate_subsets_recursive(arr, index=0, current_subset=[]):
    # 기저 조건: 모든 원소를 고려했을 때
    if index == len(arr):
        return [current_subset.copy()]
    
    # 현재 원소를 포함하지 않는 경우
    subsets = generate_subsets_recursive(arr, index + 1, current_subset)
    
    # 현재 원소를 포함하는 경우
    current_subset.append(arr[index])
    subsets.extend(generate_subsets_recursive(arr, index + 1, current_subset))
    current_subset.pop()  # 백트래킹
    
    return subsets
```

### 3. itertools를 이용한 조합 생성

```python
from itertools import combinations

def generate_subsets_itertools(arr):
    subsets = []
    for r in range(len(arr) + 1):
        subsets.extend(list(combinations(arr, r)))
    return subsets

# 특정 크기의 부분집합만 생성
def get_subsets_of_size(arr, k):
    return list(combinations(arr, k))
```

---

## 💡 부분집합 활용 문제 유형

### 1. 부분집합 합 문제
```python
def subset_sum(arr, target):
    """주어진 배열에서 합이 target인 부분집합이 존재하는지 확인"""
    n = len(arr)
    
    for i in range(1 << n):
        current_sum = 0
        subset = []
        
        for j in range(n):
            if i & (1 << j):
                current_sum += arr[j]
                subset.append(arr[j])
        
        if current_sum == target:
            return True, subset
    
    return False, []

# 예시
arr = [3, 34, 4, 12, 5, 2]
target = 9
exists, subset = subset_sum(arr, target)
print(f"합이 {target}인 부분집합 존재: {exists}")
if exists:
    print(f"해당 부분집합: {subset}")
```

### 2. 최적 부분집합 찾기
```python
def find_closest_subset_sum(arr, target):
    """목표값에 가장 가까운 부분집합의 합 찾기"""
    n = len(arr)
    closest_sum = float('inf')
    best_subset = []
    
    for i in range(1 << n):
        current_sum = 0
        subset = []
        
        for j in range(n):
            if i & (1 << j):
                current_sum += arr[j]
                subset.append(arr[j])
        
        if abs(current_sum - target) < abs(closest_sum - target):
            closest_sum = current_sum
            best_subset = subset
    
    return closest_sum, best_subset
```

### 3. 조건을 만족하는 부분집합 개수 세기
```python
def count_valid_subsets(arr, condition_func):
    """조건을 만족하는 부분집합의 개수"""
    n = len(arr)
    count = 0
    
    for i in range(1 << n):
        subset = []
        for j in range(n):
            if i & (1 << j):
                subset.append(arr[j])
        
        if condition_func(subset):
            count += 1
    
    return count

# 예시: 짝수 개의 원소를 가진 부분집합 개수
arr = [1, 2, 3, 4]
even_count = count_valid_subsets(arr, lambda x: len(x) % 2 == 0)
print(f"짝수 개의 원소를 가진 부분집합 개수: {even_count}")
```

---

## 🚀 성능 최적화 팁

### 1. 조기 종료 (Early Termination)
```python
def optimized_subset_sum(arr, target):
    n = len(arr)
    arr.sort()  # 정렬로 조기 종료 가능
    
    for i in range(1 << n):
        current_sum = 0
        subset = []
        
        for j in range(n):
            if i & (1 << j):
                current_sum += arr[j]
                subset.append(arr[j])
                
                # 목표를 초과하면 조기 종료
                if current_sum > target:
                    break
        
        if current_sum == target:
            return True, subset
    
    return False, []
```

### 2. 동적 계획법 활용
```python
def dp_subset_sum(arr, target):
    """DP를 이용한 부분집합 합 문제 해결"""
    n = len(arr)
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    
    # 합이 0인 경우는 항상 가능 (공집합)
    for i in range(n + 1):
        dp[i][0] = True
    
    for i in range(1, n + 1):
        for w in range(1, target + 1):
            # 현재 원소를 포함하지 않는 경우
            dp[i][w] = dp[i-1][w]
            
            # 현재 원소를 포함하는 경우
            if arr[i-1] <= w:
                dp[i][w] = dp[i][w] or dp[i-1][w - arr[i-1]]
    
    return dp[n][target]
```

---

## 📝 오늘의 깨달음

1. **델타 배열**은 2차원 격자에서 이동을 다룰 때 코드의 가독성과 유지보수성을 크게 향상시킵니다.

2. **부분집합**은 비트마스킹으로 효율적으로 구현할 수 있으며, 시간복잡도는 O(2^n)이므로 n이 클 때는 다른 접근법을 고려해야 합니다.

3. **조기 종료**와 **동적 계획법** 같은 최적화 기법을 함께 사용하면 성능을 크게 개선할 수 있습니다.

4. 실전에서는 문제의 제약 조건을 파악해서 적절한 알고리즘을 선택하는 것이 중요합니다.

---

## 🎯 다음 학습 계획
- 백트래킹과 부분집합의 조합
- 분할정복을 이용한 최적화
- 그래프에서의 부분집합 활용