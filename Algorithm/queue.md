# 큐(Queue) 자료구조 TIL

## 큐란?
- **FIFO(First In, First Out)** 원칙을 따르는 선형 자료구조
- 먼저 들어간 데이터가 먼저 나오는 구조
- 대기열과 같은 개념 (은행 창구 대기, 프린터 작업 대기 등)

## 큐의 기본 연산
- **Enqueue**: 큐의 뒤쪽(rear)에 데이터 추가
- **Dequeue**: 큐의 앞쪽(front)에서 데이터 제거
- **Front**: 큐의 맨 앞 요소 확인
- **Rear**: 큐의 맨 뒤 요소 확인

---

## 구현 방법별 특징

### 1. 리스트의 pop()과 append() 활용

```python
class Queue:
    def __init__(self):
        self.queue = []
    
    def enqueue(self, item):
        self.queue.append(item)  # 뒤에 추가
    
    def dequeue(self):
        if self.queue:
            return self.queue.pop(0)  # 앞에서 제거
        return None
```

**특징:**
- **장점**: 구현이 간단하고 직관적
- **단점**: `pop(0)` 연산이 O(n) 시간복잡도를 가짐 (모든 요소를 한 칸씩 앞으로 이동)
- **적합한 상황**: 데이터 크기가 작고 성능이 중요하지 않은 경우

### 2. front와 rear 인덱스 활용

```python
class Queue:
    def __init__(self, size):
        self.queue = [None] * size
        self.front = 0
        self.rear = 0
        self.size = size
        self.count = 0
    
    def enqueue(self, item):
        if self.count < self.size:
            self.queue[self.rear] = item
            self.rear = (self.rear + 1) % self.size  # 원형 큐
            self.count += 1
    
    def dequeue(self):
        if self.count > 0:
            item = self.queue[self.front]
            self.front = (self.front + 1) % self.size
            self.count -= 1
            return item
        return None
```

**특징:**
- **장점**: 모든 연산이 O(1) 시간복잡도
- **장점**: 메모리 효율적 (원형 큐로 구현 가능)
- **단점**: 고정 크기 제한
- **적합한 상황**: 성능이 중요하고 큐 크기를 미리 알 수 있는 경우

### 3. collections.deque 활용

```python
from collections import deque

class Queue:
    def __init__(self):
        self.queue = deque()
    
    def enqueue(self, item):
        self.queue.append(item)      # 오른쪽에 추가
    
    def dequeue(self):
        if self.queue:
            return self.queue.popleft()  # 왼쪽에서 제거
        return None
```

**특징:**
- **장점**: 양쪽 끝에서의 추가/제거가 모두 O(1)
- **장점**: 동적 크기 조절 가능
- **장점**: 내장 라이브러리로 안정성과 최적화가 보장됨
- **적합한 상황**: 대부분의 실제 개발 상황에서 권장

---

## 각 방법의 시간복잡도 비교

| 연산 | pop/append | front/rear | deque |
|------|------------|------------|-------|
| Enqueue | O(1) | O(1) | O(1) |
| Dequeue | O(n) | O(1) | O(1) |
| 공간복잡도 | O(n) | O(n) | O(n) |

---

## 실제 사용 시나리오

### deque를 사용하는 경우 (대부분 권장)
- 웹 서버의 요청 처리 대기열
- BFS(너비 우선 탐색) 알고리즘
- 프로세스 스케줄링
- 캐시 구현 (LRU 캐시)

### front/rear 인덱스를 사용하는 경우
- 임베디드 시스템 (메모리 제약)
- 실시간 시스템 (예측 가능한 성능 필요)
- 게임 개발 (고정 크기 버퍼)

### pop/append를 사용하는 경우
- 학습 목적
- 프로토타이핑
- 매우 작은 데이터셋

---

## 학습 포인트

1. **성능 고려사항**: `pop(0)`은 O(n)이므로 큰 데이터에서는 비효율적
2. **실무에서는 deque 사용**: Python에서 큐가 필요하면 `collections.deque` 사용
3. **메모리 vs 성능**: 고정 크기 배열은 메모리 효율적이지만 유연성이 떨어짐
4. **적절한 도구 선택**: 상황에 맞는 구현 방법을 선택하는 것이 중요

## 다음 학습 계획
- [ ] 우선순위 큐(Priority Queue) 학습
- [ ] 큐를 활용한 알고리즘 문제 풀이
- [ ] 스택과 큐의 조합 활용 사례 탐구