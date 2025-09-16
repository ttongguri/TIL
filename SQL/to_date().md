# SQL TO_DATE() 함수 TIL

## 개념
- **정의**: 문자열을 DATE 데이터 타입으로 변환하는 SQL 함수
- **주요 용도**: 문자열로 저장된 날짜 데이터를 실제 날짜 타입으로 변환

## 문법
```sql
TO_DATE(string, format)
```
- `string`: 변환할 문자열
- `format`: 날짜 형식 패턴

## 예제

### 기본 사용법
```sql
-- 표준 날짜 형식
TO_DATE('2024-03-15', 'YYYY-MM-DD')

-- 다른 구분자
TO_DATE('15/03/2024', 'DD/MM/YYYY')
TO_DATE('2024.03.15', 'YYYY.MM.DD')

-- 한글 포함
TO_DATE('2024년 3월 15일', 'YYYY"년" MM"월" DD"일"')
```

### 시간 포함
```sql
TO_DATE('2024-03-15 14:30:25', 'YYYY-MM-DD HH24:MI:SS')
TO_DATE('15-MAR-24 02:30:25 PM', 'DD-MON-YY HH:MI:SS PM')
```

## 주요 포맷 패턴
- `YYYY`: 4자리 년도
- `YY`: 2자리 년도
- `MM`: 월 (01-12)
- `MON`: 월 약어 (JAN, FEB, ...)
- `DD`: 일 (01-31)
- `HH24`: 24시간 형식 시간
- `HH`: 12시간 형식 시간
- `MI`: 분
- `SS`: 초
- `AM/PM`: 오전/오후 구분자

## 데이터베이스별 차이점
- **Oracle**: `TO_DATE()` 함수 사용
- **PostgreSQL**: `TO_DATE()`, `CAST()`, `::DATE` 사용 가능
- **MySQL**: `STR_TO_DATE()` 함수 사용
- **SQL Server**: `CONVERT()`, `CAST()` 함수 사용

## 주의사항
- 포맷 패턴이 입력 문자열과 정확히 일치해야 함
- 잘못된 날짜(예: 2월 30일)는 오류 발생
- 데이터베이스마다 지원하는 함수명과 포맷이 다름

## 실무 활용
```sql
-- 문자열 날짜 컬럼을 실제 날짜로 변환하여 조회
SELECT *
FROM orders
WHERE TO_DATE(order_date, 'YYYY-MM-DD') >= '2024-01-01';

-- 날짜 계산
SELECT 
    order_id,
    TO_DATE(order_date, 'YYYY-MM-DD') + 7 as delivery_date
FROM orders;
```