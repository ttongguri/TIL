# Python `dir()`과 `help()` 함수 완전 정복

## 개요

Python에서 객체나 모듈의 정보를 확인할 때 사용하는 두 가지 핵심 내장 함수를 알아보자.

## `dir()` 함수

### 용도
객체가 가지고 있는 **속성(attribute)과 메서드의 이름 목록**을 반환한다.

### 언제 사용하나?
- 어떤 객체에 어떤 메서드가 있는지 빠르게 확인하고 싶을 때
- 새로운 라이브러리나 모듈을 탐색할 때
- 객체의 구조를 파악하고 싶을 때

### 사용 예시

```python
# 문자열 객체의 메서드들 확인
text = "hello"
print(dir(text))
# ['capitalize', 'casefold', 'center', 'count', 'encode', ...]

# 리스트 객체의 메서드들 확인
my_list = [1, 2, 3]
print(dir(my_list))
# ['append', 'clear', 'copy', 'count', 'extend', 'index', ...]

# 모듈의 함수들 확인
import math
print(dir(math))
# ['acos', 'acosh', 'asin', 'asinh', 'atan', 'atan2', ...]

# 현재 네임스페이스의 모든 이름들 확인
print(dir())
# 현재 스코프에 정의된 모든 변수, 함수, 클래스 이름들
```

## `help()` 함수

### 용도
객체, 함수, 모듈에 대한 **상세한 도움말과 사용법**을 제공한다.

### 언제 사용하나?
- 함수나 메서드의 사용법을 자세히 알고 싶을 때
- 매개변수와 반환값에 대한 정보가 필요할 때
- docstring이나 문서를 확인하고 싶을 때

### 사용 예시

```python
# 특정 함수의 도움말 보기
help(len)
# len(obj, /)
#     Return the number of items in a container.

# 문자열 메서드의 도움말 보기
help(str.replace)
# replace(old, new[, count]) -> str
#     Return a copy with all occurrences of substring old replaced by new.

# 모듈 전체 도움말 보기
import math
help(math)
# 수학 모듈의 모든 함수와 상수에 대한 상세 설명

# 객체의 도움말 보기
help(list.append)
# append(object, /) -> None
#     Append object to the end of the list.
```

## 두 함수의 비교

| 구분 | `dir()` | `help()` |
|------|---------|----------|
| **출력 형태** | 이름 목록 (리스트) | 상세한 텍스트 설명 |
| **정보량** | 간단 (이름만) | 상세 (사용법, 매개변수, 예시) |
| **사용 시점** | "뭐가 있지?" | "어떻게 쓰지?" |
| **탐색 목적** | 빠른 개요 파악 | 깊이 있는 이해 |
| **출력 길이** | 짧음 | 길고 자세함 |

## 실전 활용 팁

### 1. 단계별 탐색 전략
```python
# 1단계: 어떤 메서드들이 있는지 확인
dir(str)

# 2단계: 관심 있는 메서드의 상세 정보 확인
help(str.split)
```

### 2. 필터링해서 보기
```python
# 언더스코어로 시작하지 않는 공개 메서드만 보기
text = "hello"
public_methods = [method for method in dir(text) if not method.startswith('_')]
print(public_methods)
```

### 3. 인터랙티브 탐색
```python
# Python 인터프리터에서
>>> import requests
>>> dir(requests)  # 먼저 전체 구조 파악
>>> help(requests.get)  # 특정 함수 사용법 확인
```

## 실무에서 언제 쓸까?

**새로운 라이브러리 학습할 때:**
```python
import pandas as pd
df = pd.DataFrame()
dir(df)  # DataFrame에 어떤 메서드들이 있나?
help(df.groupby)  # groupby는 어떻게 쓰는거지?
```

**디버깅할 때:**
```python
# 객체가 예상한 메서드를 가지고 있는지 확인
if 'append' in dir(my_object):
    my_object.append(item)
```

**API 탐색할 때:**
```python
import json
dir(json)  # json 모듈에 뭐가 있지?
help(json.dumps)  # dumps 함수 사용법은?
```

## 마무리

`dir()`과 `help()`는 Python 개발자의 필수 도구다. `dir()`로 빠르게 훑어보고, `help()`로 깊이 파고드는 습관을 기르자. 특히 새로운 라이브러리를 배울 때나 까먹은 함수 사용법을 확인할 때 정말 유용하다.

**기억할 점:** 구글링하기 전에 `help()`부터 써보자!