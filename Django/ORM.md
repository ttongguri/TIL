## Django ORM, CRUD, Field Lookups, QuerySet API

### 1. ORM (Object Relational Mapping)
- Django에서 **파이썬 객체와 데이터베이스를 매핑**해주는 도구.
- SQL을 직접 작성하지 않고도 데이터베이스 조작 가능.
- 장점:
  - 생산성: Python 코드만으로 DB 작업 가능
  - 가독성: SQL 문법을 몰라도 쉽게 데이터 접근
  - 이식성: DB 엔진 교체 시 코드 수정 최소화

---

### 2. QuerySet API
- Django ORM이 제공하는 **데이터 조회·수정·삭제를 위한 인터페이스**.
- 장점:
  - SQL을 추상화해 일관된 문법 제공
  - 지연 평가(Lazy Evaluation)로 효율적 쿼리 실행
  - 체이닝 가능 → 가독성과 재사용성 향상

```python
# 모든 데이터 가져오기
articles = Article.objects.all()

# 조건 검색
published = Article.objects.filter(status='published')

# 단일 객체 가져오기
first_article = Article.objects.get(id=1)
```

---

### 3. CRUD with ORM
- **Create** (생성)
```python
article = Article(title="ORM 공부", body="Django ORM 정리")
article.save()
```

- **Read** (조회)
```python
articles = Article.objects.all()
one_article = Article.objects.get(id=1)
```

- **Update** (수정)
```python
article = Article.objects.get(id=1)
article.title = "ORM 복습"
article.save()
```

- **Delete** (삭제)
```python
article = Article.objects.get(id=1)
article.delete()
```

---

### 4. Field Lookups
- Django ORM에서 **조건 검색 시 사용하는 문법**.
- 사용 예시:
```python
# title이 특정 값과 같은 경우
Article.objects.filter(title__exact="ORM 공부")

# title에 특정 문자열이 포함된 경우
Article.objects.filter(title__contains="ORM")

# 생성일이 특정 날짜 이후
Article.objects.filter(created_at__gte="2025-01-01")

# 아이디가 1, 2, 3 중 하나인 경우
Article.objects.filter(id__in=[1, 2, 3])
```

자주 쓰는 Lookups:
- `exact` : 값이 정확히 일치
- `iexact` : 대소문자 구분 없는 일치
- `contains`, `icontains` : 포함 여부
- `gt`, `gte`, `lt`, `lte` : 비교 연산
- `in` : 리스트 안에 포함되는지 확인

---

## 오늘의 깨달음
- ORM 덕분에 SQL을 직접 작성하지 않아도 데이터를 직관적으로 다룰 수 있다.
- QuerySet API는 체이닝과 지연 평가로 효율적인 데이터 접근을 제공한다.
- Field Lookup은 SQL의 WHERE 절을 직관적으로 표현하는 방법이며, 다양한 조건 검색이 가능하다.

