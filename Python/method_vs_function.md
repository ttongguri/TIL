## 메서드 vs 함수

### 함수 (Function)
- 독립적으로 존재하는 코드 블록
- 특정 클래스나 객체에 속하지 않음
- 전역적으로 호출 가능

```python
def add(a, b):
    return a + b

result = add(5, 3)  # 독립적으로 호출
```

### 메서드 (Method)
- 클래스나 객체에 속한 함수
- 해당 객체의 데이터에 접근하고 조작 가능
- 객체를 통해서만 호출 가능

```python
class Calculator:
    def __init__(self):
        self.result = 0
    
    def add(self, num):  # 메서드
        self.result += num
        return self.result

calc = Calculator()
calc.add(5)  # 객체를 통해 메서드 호출
```

**핵심:** 메서드는 "객체가 할 수 있는 행동", 함수는 "독립적인 작업 단위"

---

## requests.get().json() 구조 분석

### 구조 설명
```python
requests.get("https://api.example.com").json()
```

- `requests`: 모듈(라이브러리)
- `get`: requests 모듈의 **함수** (클래스 아님)
- `get()`의 반환값: **Response 객체**
- `json`: Response 객체의 **메서드**

### 메서드 체이닝의 원리
```python
# 실제로는 이렇게 동작:
response = requests.get(url)  # Response 객체 반환
data = response.json()        # Response 객체의 메서드 호출
```

### 메서드의 숨겨진 인자
모든 메서드는 자동으로 객체 자신(`self`)을 첫 번째 인자로 받음:

```python
# 이 두 방식은 동일:
response.json()           # 일반적인 호출
Response.json(response)   # 클래스에서 직접 호출
```

---

## 클래스의 구성 요소

### 속성 (Attributes) - 명사형
```python
response.text         # 응답의 텍스트 내용
response.status_code  # HTTP 상태 코드
response.headers      # HTTP 헤더 정보
```

### 메서드 (Methods) - 동사형
```python
response.json()              # JSON으로 파싱하다
response.raise_for_status()  # 에러 상태 확인하다
```

**차이점:**
- **속성**: 객체가 가지고 있는 데이터/상태 (괄호 없음)
- **메서드**: 객체가 수행할 수 있는 동작 (괄호 있음)

### 클래스 구성 요소들
1. **인스턴스 속성**: 각 객체마다 다른 값
2. **클래스 속성**: 모든 객체가 공유하는 값
3. **인스턴스 메서드**: 일반적인 메서드
4. **클래스 메서드**: `@classmethod`
5. **정적 메서드**: `@staticmethod`
6. **특수 메서드**: `__init__`, `__str__` 등
7. **프로퍼티**: `@property`

---

## requests.get().json() 내부 동작 원리

### 두 단계 처리
1. **`requests.get(url)` 실행:**
   - HTTP 요청을 보냄
   - 서버에서 응답을 받음 (예: `'{"name": "Alice"}'`)
   - 문자열 데이터를 가진 Response 객체 생성하여 반환

2. **`.json()` 메서드 실행:**
   - Response 객체 내부에서 `self.text` 가져옴
   - 내부적으로 `json.loads(self.text)` 실행
   - 문자열을 Python 딕셔너리로 변환

### 내부적으로는
```python
# 1단계: HTTP 요청 및 응답
response_text = '{"name": "Alice", "age": 30}'
response = Response(response_text)

# 2단계: JSON 파싱
import json
result = json.loads(response.text)  # 문자열 → 딕셔너리
```

---

## 💡 주요 깨달음과 느낀 점

### 1. 메서드 체이닝의 본질
처음에는 `requests.get().json()`이 마법 같았는데, 알고 보니 단순한 두 단계였다:
1. `requests.get()`이 객체를 반환
2. 그 객체의 메서드를 바로 호출

**"아하 모멘트":** 메서드는 결국 함수인데, 첫 번째 인자(self)가 자동으로 전달되는 것!

### 2. 객체지향의 직관적 이해
- **속성**: 객체가 "가진 것" (명사형, 괄호 없음)
- **메서드**: 객체가 "할 수 있는 것" (동사형, 괄호 있음)

이 구분이 명확해지니까 코드 읽기가 훨씬 쉬워졌다.

---

## 🔄 응용 예시들

### 1. 문자열 메서드 체이닝
```python
# 이제 이 코드가 완전히 이해됨
result = "  Hello World  ".strip().lower().replace(" ", "_")
# 1. "  Hello World  " (str 객체)
# 2. .strip() → "Hello World" (새로운 str 객체)
# 3. .lower() → "hello world" (새로운 str 객체)  
# 4. .replace() → "hello_world" (새로운 str 객체)
```

### 2. 리스트 메서드들
```python
numbers = [3, 1, 4, 1, 5]
numbers.sort()  # 리스트 객체가 자기 자신을 정렬하는 행동
print(numbers.count(1))  # 리스트 객체가 1의 개수를 세는 행동

# 이것도 결국:
# list.sort(numbers)
# list.count(numbers, 1) 과 동일한 내부 동작
```

### 3. 파일 객체 예시
```python
with open('file.txt', 'r') as file:
    content = file.read().strip().split('\n')
    
# 1. file.read() → 문자열 객체 반환
# 2. .strip() → 그 문자열 객체의 메서드
# 3. .split() → strip()이 반환한 문자열 객체의 메서드
```

### 4. DataFrame 체이닝 (pandas)
```python
import pandas as pd
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
result = df.dropna().reset_index(drop=True).head(2)

# 각 메서드마다 새로운 DataFrame 객체가 반환됨
```

---

## 🎯 실전 적용 포인트

### 1. 에러 디버깅이 쉬워짐
```python
# 에러가 난다면 단계별로 나누어 확인
# requests.get(url).json() 에서 에러 시:

response = requests.get(url)  # 1단계 확인
print(response.status_code)   # 응답이 정상인지 확인
data = response.json()        # 2단계 확인
```

### 2. 메서드 체이닝 설계 원칙
- 각 메서드는 적절한 객체를 반환해야 함
- 체이닝이 가능하려면 일관된 인터페이스 필요

### 3. 코드 가독성
```python
# 복잡한 체이닝보다는
user_data = requests.get(api_url).json()['users'][0]['profile']['name'].upper()

# 단계별로 나누는 게 디버깅에 좋음
response = requests.get(api_url)
data = response.json()
user_name = data['users'][0]['profile']['name']
formatted_name = user_name.upper()
```

---

## 🔗 다른 언어와의 연결점

### JavaScript와 비교
```javascript
// JavaScript도 동일한 패턴
fetch(url)
  .then(response => response.json())  // 메서드 체이닝
  .then(data => console.log(data));

// 배열 메서드 체이닝
[1, 2, 3, 4, 5]
  .filter(x => x > 2)
  .map(x => x * 2)
  .reduce((a, b) => a + b);
```

**배운점:** `requests.get(url).json()`은 결국 두 단계로 나누어져서 처리되며, json() 메서드의 self 매개변수에는 requests.get(url)의 결과인 Response 객체가 들어간다. 이 원리를 이해해야 모든 메서드 체이닝을 직관적으로 파악할 수 있다.