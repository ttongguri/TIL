# 알고리즘 기초 개념 정리

## 📚 학습 내용
- 델타(Delta) 배열을 이용한 방향 지정
- 이진 탐색 (Binary Search)
- 선택 정렬 (Selection Sort)

---

## 1. 델타(Delta) 배열 

### 개념
델타 배열은 2차원 배열에서 특정 위치로부터 상하좌우 또는 대각선 방향으로 이동할 때 사용하는 방향 벡터입니다.

### 상하좌우 방향 지정
```python
# 상, 하, 좌, 우
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

# 또는 시계방향으로
dx = [-1, 0, 1, 0]  # 상, 우, 하, 좌
dy = [0, 1, 0, -1]
```

### 대각선 포함 8방향
```python
# 상, 상우, 우, 하우, 하, 하좌, 좌, 상좌
dx = [-1, -1, 0, 1, 1, 1, 0, -1]
dy = [0, 1, 1, 1, 0, -1, -1, -1]
```

### 사용 예시
```python
def explore_directions(board, x, y):
    dx = [-1, 1, 0, 0]  # 상하좌우
    dy = [0, 0, -1, 1]
    
    for i in range(4):
        nx = x + dx[i]
        ny = y + dy[i]
        
        # 범위 체크
        if 0 <= nx < len(board) and 0 <= ny < len(board[0]):
            # 해당 방향으로 이동 가능한 경우 처리
            print(f"이동 가능: ({nx}, {ny})")
```

### 💡 핵심 포인트
- `dx[i], dy[i]`는 항상 쌍으로 사용
- 배열 범위를 벗어나지 않도록 경계 검사 필수
- 방향의 순서를 일관성 있게 정의

---

## 2. 이진 탐색 (Binary Search) 🔍

### 개념
**정렬된 배열**에서 특정 값을 찾는 효율적인 알고리즘으로, 검색 범위를 절반씩 줄여나가는 방식입니다.

### 시간 복잡도
- **O(log n)** - 매번 검색 범위가 절반으로 줄어듦

### 구현 방법

#### 반복문 버전
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1  # 찾지 못한 경우
```

#### 재귀 버전
```python
def binary_search_recursive(arr, target, left, right):
    if left > right:
        return -1
    
    mid = (left + right) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)
```

### 💡 핵심 포인트
- **정렬된 배열**에서만 사용 가능
- `left <= right` 조건 주의
- `mid = (left + right) // 2`로 중간값 계산

---

## 3. 선택 정렬 (Selection Sort) 📊

### 개념
배열에서 **최솟값을 찾아서 맨 앞으로 이동**시키는 과정을 반복하는 정렬 알고리즘입니다.

### 시간 복잡도
- **O(n²)** - 모든 경우에 동일 (최선, 평균, 최악)
- 공간 복잡도: **O(1)** - 제자리 정렬

### 동작 과정
1. 배열에서 최솟값을 찾는다
2. 최솟값을 맨 앞 요소와 교환한다
3. 정렬된 부분을 제외하고 위 과정을 반복한다

### 구현
```python
def selection_sort(arr):
    n = len(arr)
    
    for i in range(n - 1):
        # 최솟값의 인덱스를 찾기
        min_idx = i
        
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        # 최솟값을 현재 위치로 이동
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    
    return arr
```

### 예시 실행 과정
```
초기: [64, 25, 12, 22, 11]

1회전: [11, 25, 12, 22, 64]  # 11이 맨 앞으로
2회전: [11, 12, 25, 22, 64]  # 12가 두 번째로
3회전: [11, 12, 22, 25, 64]  # 22가 세 번째로
4회전: [11, 12, 22, 25, 64]  # 25가 네 번째로 (완료)
```

### 💡 핵심 포인트
- 매번 남은 배열에서 최솟값을 찾아 앞으로 이동
- 안정 정렬이 아님 (동일한 값의 순서가 바뀔 수 있음)
- 비교 횟수가 항상 동일하여 성능이 일정함

---

## 📝 오늘의 정리

1. **델타 배열**은 2D 배열에서 방향 이동을 깔끔하게 처리할 수 있는 핵심 기법
2. **이진 탐색**은 정렬된 데이터에서 O(log n)의 빠른 검색 성능 제공
3. **선택 정렬**은 직관적이지만 O(n²)로 큰 데이터에는 비효율적

### 다음에 학습할 것
- BFS/DFS에서 델타 배열 활용
- 이진 탐색의 변형 문제들 (lower_bound, upper_bound)
- 더 효율적인 정렬 알고리즘들 (퀵 정렬, 병합 정렬)