# 정렬 알고리즘과 탐색 알고리즘 TIL

## 정렬 알고리즘 (Sorting Algorithms)

### 버블 정렬 (Bubble Sort)
- **시간복잡도**: O(n²)
- **공간복잡도**: O(1)
- **원리**: 인접한 두 원소를 비교하여 큰 값을 뒤로 보내는 과정을 반복
- **특징**: 구현이 간단하지만 효율성이 낮음

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr
```

### 카운팅 정렬 (Counting Sort)
- **시간복잡도**: O(n + k) (k는 데이터 범위)
- **공간복잡도**: O(k)
- **원리**: 각 원소의 개수를 세어 정렬
- **특징**: 정수나 범위가 제한된 데이터에 효율적

```python
def counting_sort(arr):
    max_val = max(arr)
    count = [0] * (max_val + 1)
    
    # 각 원소의 개수 세기
    for num in arr:
        count[num] += 1
    
    # 결과 배열 생성
    result = []
    for i in range(len(count)):
        result.extend([i] * count[i])
    
    return result
```

## 탐색 알고리즘 (Search Algorithms)

### 완전탐색 (Brute Force)
- **시간복잡도**: 문제에 따라 다름 (보통 지수적)
- **원리**: 가능한 모든 경우를 다 확인하는 방법
- **특징**: 확실한 답을 보장하지만 시간이 오래 걸림

```python
# 예시: 배열에서 합이 target인 두 수 찾기
def two_sum_brute_force(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i+1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

### 그리드 탐색 (Grid Search)
- **용도**: 2차원 배열이나 격자에서의 탐색
- **방법**: DFS, BFS를 주로 활용
- **특징**: 상하좌우 이동, 경로 찾기 등에 사용

```python
# 예시: 2D 그리드에서 DFS
def dfs_grid(grid, visited, x, y):
    if x < 0 or x >= len(grid) or y < 0 or y >= len(grid[0]):
        return
    if visited[x][y] or grid[x][y] == 0:
        return
    
    visited[x][y] = True
    
    # 상하좌우 탐색
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    for dx, dy in directions:
        dfs_grid(grid, visited, x + dx, y + dy)
```

## 학습 포인트

### 정렬 선택 기준
- **데이터 크기가 작을 때**: 버블 정렬도 사용 가능
- **데이터 범위가 제한적일 때**: 카운팅 정렬이 매우 효율적
- **일반적인 경우**: 퀵 정렬, 머지 정렬 권장

### 탐색 전략
- **완전탐색**: 문제 크기가 작고 정확한 답이 필요할 때
- **그리드 탐색**: 2D 배열 문제에서 DFS/BFS 패턴 익히기

### 다음 학습 계획
- [ ] 퀵 정렬, 머지 정렬 학습
- [ ] 이진 탐색 알고리즘
- [ ] 동적 계획법 기초
- [ ] 그래프 탐색 심화 (위상 정렬 등)

---