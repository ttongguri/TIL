# Python Set/Dict 메서드 완전 비교 가이드

## 1. Set 메서드 3자 비교: discard vs remove vs pop

### 기본 특징 비교

| 메서드 | 대상 요소 | 에러 처리 | 반환값 | 사용 목적 |
|--------|-----------|-----------|--------|-----------|
| `discard(element)` | 지정된 요소 | 요소 없어도 에러 없음 | None | 안전한 제거 |
| `remove(element)` | 지정된 요소 | 요소 없으면 KeyError | None | 확실한 제거 |
| `pop()` | 임의의 요소 | 빈 set이면 KeyError | 제거된 요소 | 임의 요소 추출 |

### 코드 예시

```python
# 테스트용 set들
set1 = {1, 2, 3, 4, 5}
set2 = {1, 2, 3, 4, 5}
set3 = {1, 2, 3, 4, 5}

# 1. discard - 안전한 제거
set1.discard(3)        # 정상 제거
set1.discard(10)       # 없어도 에러 없음
result = set1.discard(1)  # None 반환

# 2. remove - 확실한 제거
set2.remove(3)         # 정상 제거
# set2.remove(10)      # KeyError 발생!
result = set2.remove(1)   # None 반환

# 3. pop - 임의 요소 추출
element = set3.pop()   # 임의 요소 반환
# set3.pop(2)          # TypeError! 인자 받지 않음
# empty_set.pop()      # KeyError 발생!
```

### 언제 사용할까?

- **discard**: 요소가 있을지 불확실하지만 안전하게 제거하고 싶을 때
- **remove**: 요소가 반드시 있어야 하고, 없으면 에러로 처리하고 싶을 때  
- **pop**: 어떤 요소든 상관없이 하나씩 꺼내면서 처리하고 싶을 때

## 2. set.pop() vs dict.pop() 비교

### 메서드 시그니처

```python
# Set
set.pop()                    # 인자 없음

# Dictionary  
dict.pop(key)               # 키 필수
dict.pop(key, default)      # 기본값 선택적
```

### 주요 차이점

| 특징 | set.pop() | dict.pop(key) |
|------|-----------|---------------|
| **인자** | 없음 (인자 받지 않음) | 키 필수, 기본값 선택적 |
| **대상** | 임의의 요소 | 지정된 키의 값 |
| **반환값** | 제거된 요소 | 제거된 값 |
| **에러 상황** | 빈 set → KeyError | 키 없음 → KeyError (기본값 없을 때) |
| **제어 가능성** | 없음 (랜덤) | 있음 (키로 지정) |

### 코드 비교

```python
# Set pop - 임의 요소
my_set = {'apple', 'banana', 'cherry'}
element = my_set.pop()          # 임의의 과일 하나
# my_set.pop('apple')           # TypeError!

# Dict pop - 지정된 키
my_dict = {'apple': 1, 'banana': 2, 'cherry': 3}
value = my_dict.pop('banana')           # 2 반환
value = my_dict.pop('orange', 0)        # 0 반환 (기본값)
# value = my_dict.pop('orange')         # KeyError!
```

### 설계 철학의 차이

**Set**: 
- 순서 없는 고유 요소들의 집합
- "특정 요소"를 지정할 개념적 기준이 없음
- pop()은 "아무거나 하나" 제거하는 것이 자연스러움

**Dictionary**:
- 키-값 쌍의 매핑 구조
- 키를 통해 특정 값에 명확하게 접근 가능
- pop(key)로 원하는 데이터를 정확히 제거 가능

## 3. 실무 활용 팁

### 에러 처리 패턴

```python
# 안전한 set 요소 제거
def safe_remove_from_set(s, element):
    s.discard(element)  # 가장 간단
    
def checked_remove_from_set(s, element):
    if element in s:
        s.remove(element)
        return True
    return False

# 안전한 dict 값 제거  
def safe_remove_from_dict(d, key):
    return d.pop(key, None)  # 기본값 활용
```

### 성능 특징

- 모든 메서드의 평균 시간복잡도: **O(1)**
- set.pop()이 약간 더 빠름 (해시 순회 없이 임의 선택)
- dict.pop()은 키 검색 과정 필요

### 사용 시나리오

```python
# Set 메서드 활용
def process_tasks(task_set):
    while task_set:
        task = task_set.pop()        # 순서 상관없이 처리
        process(task)

def cleanup_cache(cache_set, invalid_items):
    for item in invalid_items:
        cache_set.discard(item)      # 안전한 정리

# Dict 메서드 활용  
def get_user_setting(settings, key):
    return settings.pop(key, 'default')  # 설정 추출 + 기본값

def remove_expired(cache_dict, expired_keys):
    for key in expired_keys:
        cache_dict.pop(key, None)    # 안전한 만료 데이터 제거
```

## 4. 요약

- **Set 제거**: `discard` (안전) → `remove` (확실) → `pop` (임의)
- **제어성**: dict.pop (키 지정) > set 지정 제거 > set.pop (랜덤)
- **안전성**: discard > dict.pop(key, default) > remove > pop
- **유연성**: dict.pop > discard ≈ remove > set.pop