## Django의 Model, Field, Migration, Admin

### 1. Model
- Django에서 **데이터베이스 테이블을 표현하는 클래스**.
- `models.Model`을 상속해서 정의.
- 하나의 클래스 = 하나의 테이블, 인스턴스 = 행(row).

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)      # 문자열 필드
    body = models.TextField()                     # 긴 텍스트 필드
    created_at = models.DateTimeField(auto_now_add=True)  # 생성 시 자동 저장
```

---

### 2. Field
- **테이블 컬럼을 정의하는 속성**.
- 필드 타입에 따라 DB 컬럼 타입이 달라짐.
- 자주 사용하는 필드:
  - `CharField(max_length=n)` : 짧은 문자열
  - `TextField()` : 긴 문자열
  - `IntegerField()` : 정수
  - `BooleanField()` : True/False
  - `DateTimeField()` : 날짜와 시간
- 주요 옵션:
  - `null=True` : DB에 NULL 허용
  - `blank=True` : 폼에서 비워둘 수 있음
  - `default=value` : 기본값 설정

---

### 3. Migration
- **모델의 변경사항을 DB에 반영하는 과정**.
- Django가 모델 ↔ DB 구조를 동기화함.
- 주요 명령어:
  - `python manage.py makemigrations` → 변경사항을 마이그레이션 파일로 생성
  - `python manage.py migrate` → 생성된 파일을 DB에 적용
  - `python manage.py showmigrations` → 적용 여부 확인

---

### 4. Admin 사이트 등록
- Django는 기본 제공되는 **admin 사이트**를 통해 데이터 관리 가능.
- 모델을 admin에 등록하면 웹 UI로 데이터 생성, 수정, 삭제를 할 수 있음.
- 등록 방법:

```python
# app/admin.py
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

- `python manage.py createsuperuser` 명령어로 관리자 계정을 만든 뒤 로그인 가능.
- 등록된 모델은 admin 페이지에서 바로 관리할 수 있음.

---

## 오늘의 깨달음
- SQL을 직접 쓰지 않고, **Python 코드로 DB를 제어**할 수 있다는 점이 강력하다.
- 마이그레이션은 DB 스키마의 버전 관리와 같은 역할을 한다.
- 필드 옵션을 활용하면 유연하게 데이터 제약 조건을 줄 수 있다.
- admin 사이트를 통해 코드를 수정하지 않고도 데이터 CRUD를 손쉽게 다룰 수 있다.