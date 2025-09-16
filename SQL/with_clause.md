# SQL WITH 절(CTE) TIL

## 개념
- **정의**: CTE(Common Table Expression)를 정의하는 SQL 구문
- **용도**: 임시 결과 집합을 만들어 쿼리 내에서 재사용
- **장점**: 복잡한 쿼리의 가독성 향상, 서브쿼리 대체

## 기본 문법
```sql
WITH cte_name AS (
    SELECT 문
)
SELECT * FROM cte_name;
```

## 기본 사용 예제

### 단일 CTE
```sql
WITH sales_summary AS (
    SELECT 
        department,
        SUM(amount) as total_sales,
        COUNT(*) as order_count
    FROM sales
    GROUP BY department
)
SELECT 
    department,
    total_sales,
    order_count
FROM sales_summary
WHERE total_sales > 100000;
```

### 다중 CTE
```sql
WITH 
dept_sales AS (
    SELECT 
        department, 
        SUM(amount) as total
    FROM sales 
    GROUP BY department
),
avg_sales AS (
    SELECT AVG(total) as avg_total
    FROM dept_sales
)
SELECT 
    d.department, 
    d.total,
    ROUND(d.total / a.avg_total * 100, 2) as percentage
FROM dept_sales d
CROSS JOIN avg_sales a
WHERE d.total > a.avg_total;
```

## 재귀 CTE

### 문법
```sql
WITH RECURSIVE cte_name AS (
    -- 초기 쿼리 (Anchor member)
    SELECT ...
    
    UNION ALL
    
    -- 재귀 쿼리 (Recursive member)
    SELECT ...
    FROM table_name
    JOIN cte_name ON ...
)
```

### 계층구조 예제
```sql
WITH RECURSIVE employee_hierarchy AS (
    -- 최상위 관리자
    SELECT 
        emp_id, 
        name, 
        manager_id, 
        1 as level,
        CAST(name AS VARCHAR(1000)) as path
    FROM employees 
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- 하위 직원들
    SELECT 
        e.emp_id, 
        e.name, 
        e.manager_id, 
        h.level + 1,
        CONCAT(h.path, ' > ', e.name)
    FROM employees e
    JOIN employee_hierarchy h ON e.manager_id = h.emp_id
)
SELECT 
    emp_id,
    name,
    level,
    path
FROM employee_hierarchy
ORDER BY level, name;
```

## 실무 활용 패턴

### 1. 복잡한 집계 분석
```sql
WITH monthly_sales AS (
    SELECT 
        DATE_TRUNC('month', order_date) as month,
        SUM(amount) as total
    FROM orders
    GROUP BY DATE_TRUNC('month', order_date)
),
sales_with_growth AS (
    SELECT 
        month,
        total,
        LAG(total) OVER (ORDER BY month) as prev_month,
        ROUND(
            (total - LAG(total) OVER (ORDER BY month)) / 
            LAG(total) OVER (ORDER BY month) * 100, 2
        ) as growth_rate
    FROM monthly_sales
)
SELECT * FROM sales_with_growth
WHERE growth_rate IS NOT NULL
ORDER BY month;
```

### 2. 데이터 정제 및 변환
```sql
WITH cleaned_data AS (
    SELECT 
        customer_id,
        UPPER(TRIM(customer_name)) as customer_name,
        CASE 
            WHEN phone LIKE '%-%' THEN REPLACE(phone, '-', '')
            ELSE phone
        END as phone_clean
    FROM customers
    WHERE customer_name IS NOT NULL
),
customer_metrics AS (
    SELECT 
        c.customer_id,
        c.customer_name,
        COUNT(o.order_id) as order_count,
        SUM(o.amount) as total_spent
    FROM cleaned_data c
    LEFT JOIN orders o ON c.customer_id = o.customer_id
    GROUP BY c.customer_id, c.customer_name
)
SELECT * FROM customer_metrics
WHERE order_count > 0;
```

## 성능 고려사항
- CTE는 쿼리 실행 시마다 계산됨 (임시 테이블과 다름)
- 복잡한 CTE는 성능에 영향을 줄 수 있음
- 재귀 CTE는 무한루프 방지를 위한 제한 설정 필요

## 장단점

### 장점
- 코드 가독성 대폭 향상
- 복잡한 서브쿼리를 단순화
- 재사용 가능한 임시 결과 집합
- 재귀 쿼리로 계층구조 처리 가능

### 단점
- 매번 실행 시 다시 계산됨
- 일부 데이터베이스에서는 최적화 제한
- 과도한 중첩 시 성능 저하 가능성