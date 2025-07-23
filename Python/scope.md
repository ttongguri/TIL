# Python 함수 스코프 완벽 가이드

> **TIL**: Python의 스코프 규칙과 클로저를 활용한 고급 패턴들

## 📌 핵심 개념: LEGB 규칙

Python은 **Local → Enclosing → Global → Built-in** 순서로 변수를 탐색합니다.

```python
x = "global"  # Global scope

def outer():
    x = "enclosing"  # Enclosing scope
    
    def inner():
        x = "local"  # Local scope
        print(x)  # "local" 출력
        print(len)  # Built-in scope의 len 함수
    
    inner()
    print(x)  # "enclosing" 출력

outer()
print(x)  # "global" 출력
```

## 🔍 변수 스코프 종류별 상세 분석

### 1. 로컬 변수 (Local Variables)
- **정의**: 함수 내부에서 선언된 변수
- **생명주기**: 함수 호출 시 생성 → 함수 종료 시 소멸
- **접근 범위**: 해당 함수 내부에서만 접근 가능

```python
def my_function():
    local_var = "로컬 변수"  # 함수 내부에서만 존재
    print(local_var)  # ✅ 접근 가능

my_function()
# print(local_var)  # ❌ NameError: 함수 외부에서 접근 불가

# 함수 호출할 때마다 새로 생성
def counter():
    count = 0  # 매번 0으로 초기화
    count += 1
    return count

print(counter())  # 1
print(counter())  # 1 (이전 호출의 count는 소멸됨)
```

### 2. 전역 변수 (Global Variables)
- **정의**: 모듈(파일) 레벨에서 선언된 변수
- **생명주기**: 프로그램 시작 → 프로그램 종료까지
- **접근 범위**: 모든 함수에서 읽기 가능, 수정은 `global` 키워드 필요

```python
global_var = "전역 변수"  # 모듈 레벨에서 선언

def read_global():
    print(global_var)  # ✅ 읽기는 자유롭게 가능

def modify_global_wrong():
    global_var = "수정 시도"  # ❌ 새로운 로컬 변수 생성됨

def modify_global_correct():
    global global_var
    global_var = "올바른 수정"  # ✅ 전역 변수 수정

read_global()  # "전역 변수"
modify_global_wrong()
print(global_var)  # "전역 변수" (수정되지 않음)
modify_global_correct()
print(global_var)  # "올바른 수정"

# globals() 함수로 전역 변수들 확인
print(globals().keys())  # 모든 전역 변수/함수 이름들
```

### 3. 빌트인 변수 (Built-in Variables)
- **정의**: Python 인터프리터가 기본 제공하는 변수/함수
- **생명주기**: Python 인터프리터와 동일
- **접근 범위**: 어디서든 접근 가능 (import 불필요)

```python
# 내장 함수들 (Built-in Functions)
print(len("hello"))    # len: 길이 계산
print(type(42))        # type: 타입 확인
print(abs(-10))        # abs: 절댓값
print(max(1, 2, 3))    # max: 최댓값

# 내장 상수들 (Built-in Constants)
print(True, False)     # 불린 상수
print(None)            # None 객체

# 내장 예외들 (Built-in Exceptions)
try:
    raise ValueError("에러!")
except ValueError as e:
    print(e)

# builtins 모듈로 모든 내장 요소 확인
import builtins
print(dir(builtins))  # 모든 내장 함수/변수/예외 목록

# 내장 변수를 덮어쓸 수도 있음 (권장하지 않음)
len = 10  # ❌ 내장 함수 len을 숫자로 덮어씀
# print(len("hello"))  # ❌ TypeError: 'int' object is not callable
```

## 🔄 스코프 우선순위 실험

```python
# 모든 스코프에 같은 이름의 변수를 만들어보자
name = "Global"

def outer():
    name = "Enclosing"
    
    def inner():
        name = "Local"
        print(f"Local scope: {name}")
        
        # 각 스코프의 변수에 강제로 접근하는 방법
        print(f"Global scope: {globals()['name']}")
        
        # Built-in 변수 확인
        print(f"Built-in len function: {len}")
        
        # locals()로 현재 스코프의 모든 변수 확인
        print(f"Local variables: {list(locals().keys())}")
    
    inner()
    print(f"Enclosing scope: {name}")

outer()
print(f"Global scope: {name}")

# 출력:
# Local scope: Local
# Global scope: Global
# Built-in len function: <built-in function len>
# Local variables: ['name']
# Enclosing scope: Enclosing
# Global scope: Global
```

## 📊 변수 스코프 비교표

| 스코프 | 선언 위치 | 생명주기 | 접근 범위 | 수정 방법 |
|--------|-----------|----------|-----------|-----------|
| **Local** | 함수 내부 | 함수 호출~종료 | 해당 함수만 | 직접 할당 |
| **Enclosing** | 바깥쪽 함수 | 바깥쪽 함수 호출~종료 | 중첩 함수들 | `nonlocal` |
| **Global** | 모듈 레벨 | 프로그램 실행~종료 | 모든 함수 | `global` |
| **Built-in** | Python 내장 | 인터프리터와 동일 | 어디든지 | 덮어쓰기 가능 |

## 🎯 실무 활용 팁

```python
# 1. 네임스페이스 확인 함수
def inspect_namespace():
    local_var = "로컬"
    print("=== 네임스페이스 분석 ===")
    print(f"Local: {locals()}")
    print(f"Global keys: {list(globals().keys())}")
    print(f"Built-in count: {len(dir(__builtins__))}")

# 2. 스코프별 변수 개수 세기
def count_variables():
    local_count = len(locals())
    global_count = len(globals())
    builtin_count = len(dir(__builtins__))
    
    print(f"로컬 변수: {local_count}개")
    print(f"전역 변수: {global_count}개") 
    print(f"내장 변수: {builtin_count}개")

count_variables()
```

## ⚠️ 함정: 변수 할당과 스코프

함수 내에서 변수에 값을 할당하면 **자동으로 로컬 변수**가 됩니다!

```python
count = 0  # 전역 변수

def increment():
    count = count + 1  # ❌ UnboundLocalError!
    # Python은 할당을 보고 count를 로컬 변수로 간주
    # 하지만 사용하기 전에 정의되지 않음

def increment_correct():
    global count
    count = count + 1  # ✅ 올바른 방법
```

## 🔑 global vs nonlocal

### `global`: 전역 변수 수정

```python
counter = 0

def reset():
    global counter
    counter = 0

def increment():
    global counter
    counter += 1
```

### `nonlocal`: 바깥쪽 함수의 변수 수정

```python
def make_counter():
    count = 0
    
    def increment():
        nonlocal count
        count += 1
        return count
    
    def get_count():
        return count
    
    return increment, get_count

inc, get = make_counter()
print(inc())  # 1
print(inc())  # 2
print(get())  # 2
```

## 🧠 클로저(Closure): 함수가 환경을 "기억"한다

```python
def make_multiplier(factor):
    def multiply(number):
        return number * factor  # factor를 클로저로 캡처
    return multiply

times_3 = make_multiplier(3)
times_5 = make_multiplier(5)

print(times_3(10))  # 30
print(times_5(10))  # 50

# 클로저 변수 확인
print(times_3.__closure__[0].cell_contents)  # 3
```

## 🎯 실전 예제 1: 메모이제이션 데코레이터

```python
def memoize(func):
    cache = {}  # 데코레이터 스코프의 변수
    
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)  # 클로저로 cache 접근
        return cache[args]
    
    wrapper.cache = cache  # 캐시를 외부에서 접근 가능하게
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))  # 55
print(fibonacci.cache)  # 캐시된 결과들 확인
```

## 🏭 함수 팩토리 패턴

```python
def create_validator(min_val, max_val):
    def validate(value):
        # 외부 함수의 매개변수를 클로저로 사용
        return min_val <= value <= max_val
    
    # 검증 함수에 메타데이터 추가
    validate.min_val = min_val
    validate.max_val = max_val
    return validate

age_validator = create_validator(0, 120)
score_validator = create_validator(0, 100)

print(age_validator(25))    # True
print(score_validator(150)) # False
```

## 🚨 람다와 클로저의 흔한 함정

```python
# ❌ 잘못된 예: 모든 함수가 같은 i 값을 참조
functions = []
for i in range(3):
    functions.append(lambda x: x + i)

# 모든 함수가 i=2를 사용 (루프 종료 후의 i 값)
for func in functions:
    print(func(10))  # 12, 12, 12

# ✅ 해결 방법 1: 기본 매개변수 사용
functions = []
for i in range(3):
    functions.append(lambda x, i=i: x + i)

# ✅ 해결 방법 2: 클로저 생성 함수 사용
def make_adder(n):
    return lambda x: x + n

functions = [make_adder(i) for i in range(3)]
```

## 💡 실전 예제 2: 상태를 가진 함수

```python
def create_counter(initial=0, step=1):
    """클로저를 이용한 상태 관리"""
    count = [initial]  # 리스트로 감싸서 nonlocal 없이 수정 가능
    
    def counter(action='increment'):
        if action == 'increment':
            count[0] += step
            return count[0]
        elif action == 'decrement':
            count[0] -= step
            return count[0]
        elif action == 'reset':
            count[0] = initial
            return count[0]
        elif action == 'get':
            return count[0]
    
    return counter

# 사용 예
counter1 = create_counter(0, 2)
counter2 = create_counter(100, 5)

print(counter1('increment'))  # 2
print(counter1('increment'))  # 4
print(counter2('decrement'))  # 95
```

## ⚡ 성능 고려사항

클로저는 참조하는 변수들을 메모리에 유지하므로, 큰 객체를 다룰 때는 주의가 필요합니다:

```python
import weakref

def create_processor(large_data):
    # 큰 데이터를 약한 참조로 저장
    data_ref = weakref.ref(large_data)
    
    def process():
        data = data_ref()
        if data is not None:
            return len(data)
        return 0
    
    return process
```

## 📚 핵심 요약

- **LEGB 규칙**: Local → Enclosing → Global → Built-in 순으로 변수 탐색
- **할당 = 로컬**: 함수 내 변수 할당은 자동으로 로컬 스코프 생성
- **global/nonlocal**: 상위 스코프 변수 수정시 필수
- **클로저**: 함수가 정의된 환경의 변수를 "기억"하는 강력한 메커니즘
- **메모리 주의**: 클로저는 참조 변수들을 메모리에 유지

## 🔗 활용처

- 데코레이터 패턴
- 함수 팩토리
- 상태 관리
- 콜백 함수
- 함수형 프로그래밍

---

*Python의 스코프 시스템은 클로저와 데코레이터 같은 강력한 패턴을 가능하게 하는 핵심 메커니즘이다!*