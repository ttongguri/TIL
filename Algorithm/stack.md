# 스택(Stack) 자료구조 정리

## 🤔 의문점
"스택을 배웠는데 그냥 리스트랑 똑같아 보이는데? 어떻게 스택이라고 할 수 있지?"

## 📚 스택이란?

**스택(Stack)**은 **LIFO(Last In, First Out)** 원칙을 따르는 추상 자료형(ADT)
- 마지막에 들어간 데이터가 가장 먼저 나오는 구조
- 접시를 쌓아놓은 것처럼, 맨 위에서만 데이터를 넣고 뺄 수 있음

## 🔍 핵심 개념: 추상 자료형 vs 구현체

### 추상 자료형(Abstract Data Type, ADT)
- **개념적인 명세**: "어떤 연산들을 제공할 것인가?"
- 스택은 `push`, `pop`, `peek`, `isEmpty` 등의 연산을 정의
- **구현 방법은 상관없음**

### 구현체(Implementation)
- 실제로 메모리에서 어떻게 저장하고 처리할 것인가
- 배열, 연결리스트, 동적배열(리스트) 등으로 구현 가능

## 💡 왜 리스트로 스택을 구현해도 "스택"인가?

```python
# 이것은 리스트를 '스택처럼' 사용하는 것
stack = []
stack.append(1)  # push 연산
stack.append(2)  # push 연산
stack.pop()      # pop 연산 (2가 나옴)

# 하지만 리스트는 스택이 아닌 다른 연산도 가능
stack[0]         # 중간 요소 접근 (스택 원칙 위반!)
stack.insert(1, 5)  # 중간에 삽입 (스택 원칙 위반!)
```

**핵심**: 리스트로 스택을 **구현**할 수 있지만, 리스트 자체는 스택이 아님!

## 🛠️ 진정한 스택 구현 방법들

### 1. 클래스로 스택 인터페이스 제한하기

```python
class Stack:
    def __init__(self):
        self._items = []  # private 리스트 사용
    
    def push(self, item):
        """스택에 원소 추가"""
        self._items.append(item)
    
    def pop(self):
        """스택에서 원소 제거 및 반환"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self._items.pop()
    
    def peek(self):
        """맨 위 원소 확인 (제거하지 않음)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self._items[-1]
    
    def is_empty(self):
        """스택이 비어있는지 확인"""
        return len(self._items) == 0
    
    def size(self):
        """스택 크기 반환"""
        return len(self._items)

# 사용 예시
stack = Stack()
stack.push(1)
stack.push(2)
print(stack.pop())  # 2
print(stack.peek()) # 1
```

### 2. collections.deque 활용

```python
from collections import deque

class Stack:
    def __init__(self):
        self._items = deque()
    
    def push(self, item):
        self._items.append(item)
    
    def pop(self):
        if not self._items:
            raise IndexError("Stack is empty")
        return self._items.pop()
    
    def peek(self):
        if not self._items:
            raise IndexError("Stack is empty")
        return self._items[-1]
    
    def is_empty(self):
        return len(self._items) == 0
```

### 3. 연결리스트로 구현

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedStack:
    def __init__(self):
        self.top = None
        self.size = 0
    
    def push(self, data):
        new_node = Node(data)
        new_node.next = self.top
        self.top = new_node
        self.size += 1
    
    def pop(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        data = self.top.data
        self.top = self.top.next
        self.size -= 1
        return data
    
    def peek(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.top.data
    
    def is_empty(self):
        return self.top is None
```

## 🎯 스택의 주요 활용 사례

1. **함수 호출 관리**: 콜 스택(Call Stack)
2. **괄호 검사**: `()`, `{}`, `[]` 매칭 확인
3. **역순 출력**: 문자열이나 배열 뒤집기
4. **계산기 구현**: 중위표기법 → 후위표기법 변환
5. **브라우저 뒤로가기**: 방문 기록 관리
6. **실행 취소(Undo)**: 에디터, 게임 등

## 💭 결론

> **"그냥 내가 그렇게 쓰기 나름?"** → **아니다.**

1. **개념적으로** 스택은 명확한 원칙(LIFO)을 가진 추상 자료형
2. **구현적으로** 리스트를 사용할 수 있지만, 스택의 원칙을 지켜야 함
3. **실무적으로** 클래스나 모듈로 인터페이스를 제한하는 것이 바람직
4. **언어적으로** Python의 `list`는 동적배열이지 스택이 아님

**핵심 깨달음**: 자료구조는 "무엇으로 만드느냐"가 아니라 "어떤 원칙으로 사용하느냐"가 중요!

## 📝 오늘 배운 것
- 추상 자료형과 구현체의 차이점
- 스택의 진정한 의미와 원칙
- 올바른 스택 구현 방법들
- 단순히 "리스트를 스택처럼 쓰는 것"과 "진정한 스택"의 차이점