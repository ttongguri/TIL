# SQL 조인과 데이터 집합 레벨 TIL

## 핵심 개념

### 데이터 집합 레벨 (Data Set Level)
- **M 집합 레벨**: 여러 개의 레코드를 가진 테이블 (예: 주문 테이블, 상품 테이블)
- **1 집합 레벨**: 단일 레코드 또는 고유한 식별자를 가진 테이블 (예: 고객 테이블, 카테고리 테이블)

### 1:M 조인 시 집합 레벨 변화 규칙
**조인 결과는 항상 M 집합 레벨을 유지한다**

```sql
-- 예시: 고객(1) ⟵ 주문(M) 조인
SELECT c.customer_name, o.order_date, o.amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;

-- 결과: 주문 테이블의 M 집합 레벨을 유지
-- 한 고객이 여러 주문을 가진 경우, 고객 정보가 중복되어 나타남
```

## 테이블 관계의 올바른 이해

### 마스터-액션 관계
- **마스터 테이블**: 기준이 되는 정보를 담은 테이블 (예: 고객, 상품, 카테고리)
- **액션 테이블**: 활동이나 거래 정보를 담은 테이블 (예: 주문, 결제, 로그)

### 동격 관계의 중요성
> **주의**: 부모-자식 관계라는 표현은 피하자!

**이유:**
- 조인하는 두 테이블은 본질적으로 동격이다
- 테이블 간에는 진정한 종속성이 없다
- 외래키는 참조 무결성을 위한 제약일 뿐, 계층 구조를 의미하지 않는다

## 실제 예시와 패턴

### 1:M 조인 패턴
```sql
-- 고객별 주문 내역 조회
SELECT 
    c.customer_name,
    c.email,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_name, o.order_date;
```

**결과 특징:**
- 고객 정보(customer_name, email)가 주문 수만큼 중복
- 최종 결과는 주문(M) 집합 레벨을 따름

### M:M 조인 패턴
```sql
-- 상품별 주문 내역 (다대다 관계)
SELECT 
    p.product_name,
    o.order_date,
    oi.quantity,
    oi.unit_price
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
JOIN orders o ON oi.order_id = o.order_id;
```

## 집합 레벨 관리 전략

### 1. 집계를 통한 레벨 조정
```sql
-- M 레벨을 1 레벨로 집계
SELECT 
    c.customer_name,
    COUNT(o.order_id) as order_count,
    SUM(o.total_amount) as total_spent
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

### 2. 서브쿼리를 활용한 레벨 제어
```sql
-- 각 고객의 최신 주문만 조회 (1:1 관계 유지)
SELECT 
    c.customer_name,
    latest_order.order_date,
    latest_order.total_amount
FROM customers c
JOIN (
    SELECT 
        customer_id,
        order_date,
        total_amount,
        ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) as rn
    FROM orders
) latest_order ON c.customer_id = latest_order.customer_id AND latest_order.rn = 1;
```

## 성능 고려사항

### 조인 순서의 중요성
- 작은 테이블을 먼저 조인
- 인덱스가 있는 컬럼으로 조인
- M 집합 레벨이 커질수록 성능 영향 증가

### 인덱스 전략
```sql
-- 조인 성능 최적화를 위한 인덱스
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
```

## 주의사항

1. **데이터 중복 인지**: 1:M 조인 시 1 쪽 데이터가 중복됨을 항상 인지
2. **카운트 주의**: 단순 COUNT(*)는 중복된 레코드를 포함
3. **집계 함수 사용 시**: GROUP BY 없이 사용하면 예상과 다른 결과
4. **NULL 처리**: LEFT/RIGHT JOIN 시 NULL 값 처리 방안 고려

## 정리

- **핵심 원칙**: 1:M 조인의 결과는 항상 M 집합 레벨을 따른다
- **관계 인식**: 테이블은 동격 관계이며, 마스터-액션의 역할 구분만 있을 뿐
- **성능 최적화**: 조인 순서와 인덱스 전략이 중요
- **데이터 품질**: 중복과 NULL 처리에 주의하여 정확한 결과 도출