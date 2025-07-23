# Lambda 함수 활용법 TIL

## Lambda 함수란?
- 이름 없는 익명 함수
- 간단한 함수를 한 줄로 표현할 때 유용
- `lambda 매개변수: 표현식` 형태로 사용

## Lambda가 빛나는 상황들

### 1. map() 함수와 함께
```python
# 일반 함수 사용
def square(x):
    return x ** 2

numbers = [1, 2, 3, 4, 5]
squared = list(map(square, numbers))

# lambda 사용 - 더 간결!
squared = list(map(lambda x: x ** 2, numbers))
# [1, 4, 9, 16, 25]
```

### 2. filter() 함수와 함께
```python
# 짝수만 필터링
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = list(filter(lambda x: x % 2 == 0, numbers))
# [2, 4, 6, 8, 10]

# 특정 조건 필터링
words = ['apple', 'banana', 'cherry', 'date']
long_words = list(filter(lambda word: len(word) > 5, words))
# ['banana', 'cherry']
```

### 3. sorted() 함수의 key 매개변수
```python
# 문자열 길이로 정렬
words = ['apple', 'pie', 'banana', 'cherry']
sorted_by_length = sorted(words, key=lambda x: len(x))
# ['pie', 'apple', 'banana', 'cherry']

# 튜플의 특정 요소로 정렬
students = [('Alice', 85), ('Bob', 90), ('Charlie', 78)]
sorted_by_score = sorted(students, key=lambda x: x[1])
# [('Charlie', 78), ('Alice', 85), ('Bob', 90)]
```

### 4. max(), min() 함수와 함께
```python
# 가장 긴 문자열 찾기
words = ['apple', 'banana', 'cherry']
longest = max(words, key=lambda x: len(x))
# 'banana'

# 딕셔너리에서 값 기준으로 최대값 찾기
scores = {'Alice': 85, 'Bob': 90, 'Charlie': 78}
top_student = max(scores, key=lambda x: scores[x])
# 'Bob'
```

### 5. reduce() 함수와 함께
```python
from functools import reduce

# 모든 수의 곱 계산
numbers = [1, 2, 3, 4, 5]
product = reduce(lambda x, y: x * y, numbers)
# 120

# 문자열 연결
words = ['Hello', 'World', '!']
sentence = reduce(lambda x, y: x + ' ' + y, words)
# 'Hello World !'
```

### 6. 이벤트 처리나 콜백 함수
```python
# GUI 프로그래밍에서
button.on_click(lambda: print("버튼이 클릭되었습니다!"))

# 간단한 변환 함수
def apply_operation(numbers, operation):
    return [operation(x) for x in numbers]

# 사용 예시
result = apply_operation([1, 2, 3], lambda x: x * 3 + 1)
# [4, 7, 10]
```

### 7. 딕셔너리나 리스트 컴프리헨션과 조합
```python
# 조건부 변환
numbers = [1, 2, 3, 4, 5]
processed = [(lambda x: x**2 if x % 2 == 0 else x**3)(x) for x in numbers]
# [1, 4, 27, 16, 125]
```

## Lambda 사용 시 주의사항

### 좋은 사용 예시 ✅
- 간단한 한 줄 표현식
- 함수형 프로그래밍 도구들과 조합
- 일회성 함수가 필요할 때

### 피해야 할 경우 ❌
```python
# 너무 복잡한 lambda (가독성 떨어짐)
complex_lambda = lambda x: x**2 + 2*x + 1 if x > 0 else -x**2 + 3*x - 2 if x < -5 else 0

# 이런 경우는 일반 함수로 작성하는 것이 좋음
def complex_function(x):
    if x > 0:
        return x**2 + 2*x + 1
    elif x < -5:
        return -x**2 + 3*x - 2
    else:
        return 0
```

## 핵심 포인트
- Lambda는 **간결함**이 목적
- **함수형 프로그래밍** 스타일과 잘 어울림
- **일회성 함수**가 필요할 때 유용
- 복잡하면 일반 함수를 사용하자

## 실무에서 자주 쓰이는 패턴
```python
# 데이터 전처리
data = [{'name': 'Alice', 'age': 25}, {'name': 'Bob', 'age': 30}]
ages = list(map(lambda x: x['age'], data))

# 조건부 필터링
filtered_data = list(filter(lambda x: x['age'] > 25, data))

# 정렬
sorted_data = sorted(data, key=lambda x: x['name'])
```

## 실무 활용 사례 심화

### JSON/API 데이터 처리
```python
# API 응답에서 특정 필드만 추출
api_response = [
    {'id': 1, 'name': '김철수', 'email': 'kim@example.com', 'status': 'active'},
    {'id': 2, 'name': '이영희', 'email': 'lee@example.com', 'status': 'inactive'},
    {'id': 3, 'name': '박민수', 'email': 'park@example.com', 'status': 'active'}
]

# 이름만 추출
names = list(map(lambda user: user['name'], api_response))
# ['김철수', '이영희', '박민수']

# 활성 사용자의 이메일만 추출
active_emails = list(map(lambda user: user['email'], 
                        filter(lambda user: user['status'] == 'active', api_response)))
# ['kim@example.com', 'park@example.com']

# 중첩된 JSON 데이터 처리
products = [
    {'name': '노트북', 'price': {'amount': 1000000, 'currency': 'KRW'}},
    {'name': '마우스', 'price': {'amount': 50000, 'currency': 'KRW'}}
]

prices = list(map(lambda p: p['price']['amount'], products))
# [1000000, 50000]
```

### 데이터 변환 및 가공
```python
# 로그 데이터 처리
logs = [
    '2024-01-01 ERROR: 데이터베이스 연결 실패',
    '2024-01-01 INFO: 서버 시작됨',
    '2024-01-01 ERROR: 메모리 부족'
]

# ERROR 로그만 필터링
error_logs = list(filter(lambda log: 'ERROR' in log, logs))

# 날짜 부분만 추출
dates = list(map(lambda log: log.split()[0], logs))
# ['2024-01-01', '2024-01-01', '2024-01-01']
```

## 개인적인 느낀점과 경험

### 🎯 Lambda의 진짜 장점
- **코드 간결성**: 3-4줄 함수를 1줄로 압축
- **즉시성**: 그 자리에서 바로 로직 확인 가능
- **함수형 스타일**: map, filter 등과 자연스럽게 연결
- **데이터 파이프라인**: 여러 변환을 체인처럼 연결하기 좋음

### ⚠️ 실무에서 주의해야 할 점
```python
# ❌ 이런 복잡한 lambda는 피하자
complex_transform = lambda x: x['data']['items'][0]['value'] if 'data' in x and x['data'] and 'items' in x['data'] and len(x['data']['items']) > 0 and 'value' in x['data']['items'][0] else None

# ✅ 이렇게 함수로 분리하는 게 훨씬 좋다
def safe_extract_value(item):
    try:
        return item['data']['items'][0]['value']
    except (KeyError, IndexError, TypeError):
        return None

# 사용
values = list(map(safe_extract_value, data))
```

### 💡 실무 팁
1. **한 줄 규칙**: lambda가 한 줄을 넘어가면 함수로 분리
2. **가독성 우선**: 동료가 봤을 때 바로 이해할 수 있는 수준으로
3. **에러 처리**: 복잡한 데이터 구조에서는 try-catch가 있는 함수 사용

### 🔄 발전 과정

API 데이터 처리나 ETL 작업에서 lambda의 진가가 드러난다고 한다. 실무 프로젝트에 잘 적용해보고싶다.
복잡한 중첩 반복문 대신 함수형 스타일로 데이터를 변환하면 코드의 의도가 훨씬 명확해진다. 상황에 맞게 잘 활용하면 좋겠다.