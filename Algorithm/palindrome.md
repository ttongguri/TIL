# 회문(Palindrome) 알고리즘 TIL

## 개념 정리
회문(회문, Palindrome)은 앞에서 읽어도 뒤에서 읽어도 같은 문자열을 의미한다.

예시: `토마토`, `racecar`, `madam`, `12321`

## 구현 방법 비교

### 1. For문 + 역인덱싱 방식 (기본적인 접근)
```python
def is_palindrome_basic(s):
    """기본적인 for문과 역인덱싱을 사용한 회문 검사"""
    n = len(s)
    for i in range(n):
        if s[i] != s[n-1-i]:
            return False
    return True

# 시간복잡도: O(n)
# 공간복잡도: O(1)
```

### 2. While문 + 투 포인터 방식 (최적화된 접근) ⭐
```python
def is_palindrome_optimized(s):
    """투 포인터를 사용한 최적화된 회문 검사"""
    left, right = 0, len(s) - 1
    
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    
    return True

# 시간복잡도: O(n/2) → O(n)
# 공간복잡도: O(1)
# 실제로는 절반만 검사하므로 성능상 이점
```

## 성능 비교

### 최악의 경우 (회문이 아닌 경우)
- **기본 방식**: 전체 문자열을 모두 확인
- **투 포인터 방식**: 첫 번째 불일치에서 즉시 종료

### 최선의 경우 (회문인 경우)
- **기본 방식**: 전체 문자열 확인 필요
- **투 포인터 방식**: 절반만 확인하면 됨

## 실제 응용 사례

### 1. 유효한 회문 판별 (LeetCode 125)
```python
def is_valid_palindrome(s):
    """알파벳과 숫자만 고려하여 회문 판별"""
    # 전처리: 알파벳, 숫자만 추출하고 소문자로 변환
    cleaned = ''.join(char.lower() for char in s if char.isalnum())
    
    left, right = 0, len(cleaned) - 1
    while left < right:
        if cleaned[left] != cleaned[right]:
            return False
        left += 1
        right -= 1
    return True

# 예시: "A man, a plan, a canal: Panama" → True
```

### 2. 가장 긴 회문 부분 문자열 (LeetCode 5)
```python
def longest_palindrome(s):
    """중심 확장 알고리즘으로 가장 긴 회문 찾기"""
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
    
    start = 0
    max_len = 0
    
    for i in range(len(s)):
        # 홀수 길이 회문 (중심이 한 글자)
        len1 = expand_around_center(i, i)
        # 짝수 길이 회문 (중심이 두 글자 사이)
        len2 = expand_around_center(i, i + 1)
        
        current_max = max(len1, len2)
        if current_max > max_len:
            max_len = current_max
            start = i - (current_max - 1) // 2
    
    return s[start:start + max_len]
```

### 3. 회문 만들기 (최소 삽입)
```python
def min_insertions_for_palindrome(s):
    """문자열을 회문으로 만들기 위한 최소 삽입 횟수"""
    left, right = 0, len(s) - 1
    insertions = 0
    
    while left < right:
        if s[left] == s[right]:
            left += 1
            right -= 1
        else:
            # 왼쪽 끝에 오른쪽 문자 삽입
            insertions += 1
            left += 1
    
    return insertions
```

### 4. 회문 연결 리스트 검사
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def is_palindrome_linkedlist(head):
    """연결 리스트가 회문인지 확인"""
    if not head or not head.next:
        return True
    
    # 1. 중간 지점 찾기 (slow-fast pointer)
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    
    # 2. 후반부 뒤집기
    def reverse_list(node):
        prev = None
        while node:
            next_node = node.next
            node.next = prev
            prev = node
            node = next_node
        return prev
    
    second_half = reverse_list(slow.next)
    
    # 3. 투 포인터로 비교
    first_half = head
    while second_half:
        if first_half.val != second_half.val:
            return False
        first_half = first_half.next
        second_half = second_half.next
    
    return True
```

## 추가 최적화 기법

### 1. 해시를 이용한 빠른 비교
```python
def is_palindrome_hash(s):
    """해시값을 이용한 회문 검사 (롤링 해시)"""
    n = len(s)
    if n <= 1:
        return True
    
    base = 31
    mod = 10**9 + 7
    
    # 앞에서부터 해시값 계산
    forward_hash = 0
    # 뒤에서부터 해시값 계산  
    backward_hash = 0
    power = 1
    
    for i in range(n // 2):
        # 앞쪽 문자
        forward_hash = (forward_hash * base + ord(s[i])) % mod
        # 뒤쪽 문자
        backward_hash = (backward_hash + ord(s[n-1-i]) * power) % mod
        power = (power * base) % mod
    
    return forward_hash == backward_hash and s[n//2-1] == s[n//2] if n % 2 == 0 else True
```

## 핵심 포인트
1. **투 포인터 방식**이 기본 for문보다 효율적
2. **조기 종료**로 불필요한 연산 방지
3. **절반만 검사**하면 되므로 성능 향상
4. **다양한 변형 문제**에 응용 가능

## 시간복잡도 정리
- 기본 회문 검사: O(n/2) ≈ O(n)
- 가장 긴 회문: O(n²)
- 연결 리스트 회문: O(n)
- 해시 기반 검사: O(n)

투 포인터 패턴은 회문뿐만 아니라 **이진 탐색**, **정렬된 배열에서의 합 찾기** 등 다양한 알고리즘에서 활용되는 강력한 기법이다!