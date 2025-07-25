# 파이썬 딕셔너리 키 이름 변경하기 - TIL

## 문제 상황
파이썬 딕셔너리에서 value는 그대로 유지하면서 key 이름만 바꾸고 싶을 때가 있다. 하지만 딕셔너리에는 키를 직접 변경하는 메서드가 없다.

## 해결 방법: pop() 메서드 활용

### 기본 패턴
```python
# 기존 딕셔너리
my_dict = {'old_key': 'value'}

# 키 이름 변경
my_dict['new_key'] = my_dict.pop('old_key')

print(my_dict)  # {'new_key': 'value'}
```

### 작동 원리
1. `pop('old_key')`: 기존 키를 삭제하면서 해당 값을 반환
2. `my_dict['new_key'] = ...`: 반환된 값을 새로운 키로 할당

## 응용 예시

### 1. 사용자 정보 딕셔너리 키 정규화
```python
# API에서 받은 사용자 데이터 (키 이름이 일관성이 없음)
user_data = {
    'userName': '김철수',
    'user_email': 'kim@email.com',
    'userAge': 25
}

# 키 이름을 snake_case로 통일
user_data['user_name'] = user_data.pop('userName')
print(user_data)
# {'user_email': 'kim@email.com', 'userAge': 25, 'user_name': '김철수'}
```

### 2. 설정 파일 키 이름 업데이트
```python
# 구버전 설정 파일
config = {
    'db_host': 'localhost',
    'db_port': 5432,
    'debug_mode': True
}

# 새로운 명명 규칙에 맞게 변경
config['database_host'] = config.pop('db_host')
config['database_port'] = config.pop('db_port')
config['debug_enabled'] = config.pop('debug_mode')

print(config)
# {'database_host': 'localhost', 'database_port': 5432, 'debug_enabled': True}
```

### 3. 여러 키를 한번에 변경하는 함수
```python
def rename_keys(dictionary, key_mapping):
    """딕셔너리의 키 이름을 매핑에 따라 변경"""
    for old_key, new_key in key_mapping.items():
        if old_key in dictionary:
            dictionary[new_key] = dictionary.pop(old_key)
    return dictionary

# 사용 예시
student_info = {
    'name': '이영희',
    'grade': 'A+',
    'subject': '수학'
}

key_mapping = {
    'name': 'student_name',
    'grade': 'final_grade',
    'subject': 'course_name'
}

renamed_info = rename_keys(student_info, key_mapping)
print(renamed_info)
# {'student_name': '이영희', 'final_grade': 'A+', 'course_name': '수학'}
```

### 4. JSON 데이터 전처리
```python
# 외부 API에서 받은 JSON 데이터
api_response = {
    'userId': 123,
    'userName': 'john_doe',
    'userProfile': {
        'firstName': 'John',
        'lastName': 'Doe'
    }
}

# 데이터베이스 스키마에 맞게 키 이름 변경
api_response['user_id'] = api_response.pop('userId')
api_response['username'] = api_response.pop('userName')

# 중첩된 딕셔너리도 처리
profile = api_response['userProfile']
profile['first_name'] = profile.pop('firstName')
profile['last_name'] = profile.pop('lastName')

print(api_response)
# {'user_id': 123, 'username': 'john_doe', 'userProfile': {'first_name': 'John', 'last_name': 'Doe'}}
```

### 5. 안전한 키 변경 (키가 존재하지 않을 경우 처리)
```python
def safe_rename_key(dictionary, old_key, new_key, default_value=None):
    """키가 존재하지 않아도 에러가 발생하지 않는 안전한 키 변경"""
    if old_key in dictionary:
        dictionary[new_key] = dictionary.pop(old_key)
    elif default_value is not None:
        dictionary[new_key] = default_value
    return dictionary

# 사용 예시
data = {'existing_key': 'value'}

# 존재하는 키 변경
safe_rename_key(data, 'existing_key', 'new_existing_key')

# 존재하지 않는 키 변경 (기본값 설정)
safe_rename_key(data, 'non_existing_key', 'new_key', 'default_value')

print(data)
# {'new_existing_key': 'value', 'new_key': 'default_value'}
```

## 주의사항
- `pop()` 메서드는 키가 존재하지 않으면 `KeyError`를 발생시킴
- 안전하게 사용하려면 `pop(key, default_value)` 형태로 기본값을 지정하거나 사전에 키 존재 여부를 확인
- 키 변경 순서가 중요한 경우, 딕셔너리의 순서를 고려해야 함 (Python 3.7+에서는 삽입 순서 보장)

## 정리
`pop()` 메서드를 활용한 키 이름 변경은 간단하면서도 효과적인 방법이다. 특히 API 데이터 전처리, 설정 파일 업데이트, 데이터 정규화 등의 상황에서 유용하게 활용할 수 있다.