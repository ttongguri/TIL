# 복잡도 분석과 표준 입출력 방법


## 📚 학습 내용

### 1. 복잡도 분석 (Complexity Analysis)

#### 시간 복잡도 (Time Complexity)
알고리즘이 실행되는 데 필요한 시간을 입력 크기에 대한 함수로 나타낸 것

**Big-O 표기법의 주요 복잡도**
- **O(1)** - 상수 시간: 입력 크기와 관계없이 일정한 시간
- **O(log n)** - 로그 시간: 이진 탐색, 균형 트리 탐색
- **O(n)** - 선형 시간: 배열 순회, 단순 탐색
- **O(n log n)** - 선형 로그 시간: 효율적인 정렬 알고리즘 (병합정렬, 힙정렬)
- **O(n²)** - 제곱 시간: 중첩 반복문, 버블정렬
- **O(2ⁿ)** - 지수 시간: 피보나치 수열 (순수 재귀)

#### 공간 복잡도 (Space Complexity)
알고리즘이 실행되는 데 필요한 메모리 공간을 입력 크기에 대한 함수로 나타낸 것

**예시**
```python
# O(1) 공간 복잡도
def sum_array(arr):
    total = 0  # 상수 공간
    for num in arr:
        total += num
    return total

# O(n) 공간 복잡도
def reverse_array(arr):
    return arr[::-1]  # 새로운 배열 생성
```

#### 복잡도 분석 팁
1. **최악의 경우**를 기준으로 분석
2. **상수는 무시**: O(2n) → O(n)
3. **최고차항만 고려**: O(n² + n) → O(n²)
4. **중첩 반복문**은 곱셈으로 계산

### 2. Python 표준 입출력 방법

#### 기본 입력
```python
# 단일 값 입력
n = int(input())
text = input()

# 여러 값 한 줄 입력
a, b = map(int, input().split())

# 리스트로 입력받기
numbers = list(map(int, input().split()))
```

#### 빠른 입력 (대량 데이터)
```python
import sys
input = sys.stdin.readline

n = int(input())
for _ in range(n):
    data = input().strip()  # 개행문자 제거 필요
```

#### 파일 입력 (input.txt)
```python
# 방법 1: 파일 직접 읽기
with open('input.txt', 'r') as f:
    n = int(f.readline())
    a, b = map(int, f.readline().split())
    numbers = list(map(int, f.readline().split()))

# 방법 2: sys.stdin을 파일로 리다이렉션
import sys
sys.stdin = open('input.txt', 'r')

# 이후 일반적인 input() 사용 가능
n = int(input())
a, b = map(int, input().split())

# 방법 3: 파일 전체 내용 읽기
with open('input.txt', 'r') as f:
    lines = f.readlines()
    n = int(lines[0])
    a, b = map(int, lines[1].split())
```

#### 기본 출력
```python
# 단순 출력
print("Hello World")
print(42)

# 여러 값 출력
print(a, b)  # 공백으로 구분
print(a, b, sep=', ')  # 구분자 지정

# 개행 제어
print("Hello", end=' ')
print("World")  # 결과: "Hello World"
```

#### 파일 출력
```python
# 방법 1: 파일에 직접 쓰기
with open('output.txt', 'w') as f:
    f.write(f"결과: {result}\n")

# 방법 2: print의 file 매개변수 사용
with open('output.txt', 'w') as f:
    print("결과:", result, file=f)

# 방법 3: sys.stdout 리다이렉션
import sys
sys.stdout = open('output.txt', 'w')
print("결과:", result)  # 파일에 출력됨
sys.stdout.close()
```

### 3. 실제 적용 예시

#### 예시 1: 배열에서 최댓값 찾기
```python
def find_max(arr):
    max_val = arr[0]  # O(1) 공간
    for num in arr:   # O(n) 시간
        if num > max_val:
            max_val = num
    return max_val

# 시간 복잡도: O(n)
# 공간 복잡도: O(1)
```

#### 예시 2: 이중 반복문
```python
def print_pairs(arr):
    for i in range(len(arr)):      # n번
        for j in range(len(arr)):  # n번
            print(arr[i], arr[j])
            
# 시간 복잡도: O(n²)
# 공간 복잡도: O(1)
```

## 🔑 핵심 포인트

### 복잡도 분석
- Big-O 표기법으로 알고리즘의 효율성을 측정
- 시간과 공간 모두 고려해야 함
- 입력 크기가 클 때의 성능이 중요

### 입출력 최적화
- **Python**: `sys.stdin.readline()` 사용
- **Java**: `BufferedReader` + `StringBuilder` 조합
- **C++**: `ios_base::sync_with_stdio(false)` 설정

## 📝 추가 학습이 필요한 부분
- [ ] 분할정복 알고리즘의 복잡도 분석
- [ ] 동적 계획법에서의 공간 최적화
- [ ] 파일 입출력 방법

## 🎯 오늘의 한마디
효율적인 알고리즘 설계는 문제 해결의 첫걸음이다. 복잡도를 이해하고 적절한 입출력 방법을 선택하는 것이 성능 향상의 핵심!