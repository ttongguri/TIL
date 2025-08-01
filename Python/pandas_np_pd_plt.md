# Pandas/NumPy/Matplotlib 데이터 타입 트러블슈팅 TIL

## 오늘 겪은 문제들

### 1. 데이터 타입 불일치로 인한 오류
DataFrame 내부 데이터의 형태가 예상과 달라서 여러 오류 발생

**문제 상황:**
- CSV 파일에서 숫자 데이터가 문자열로 읽혀옴
- 날짜 데이터가 object 타입으로 인식됨
- NaN 값 때문에 연산 오류

**해결 방법:**
```python
# 데이터 타입 확인
df.dtypes
df.info()

# 타입 변환
df['column'] = pd.to_numeric(df['column'], errors='coerce')
df['date'] = pd.to_datetime(df['date'])

# NaN 처리
df.dropna()  # 또는
df.fillna(0)
```

### 2. 그래프가 의도한 대로 나오지 않는 문제

**문제 상황:**
- 카테고리형 데이터가 숫자로 인식되어 연속형 그래프 생성
- x축 순서가 뒤죽박죽
- 한글 폰트 깨짐

**해결 방법:**
```python
# 카테고리 순서 지정
df['category'] = pd.Categorical(df['category'], 
                               categories=['월', '화', '수', '목', '금'],
                               ordered=True)

# 한글 폰트 설정
plt.rcParams['font.family'] = 'DejaVu Sans'
plt.rcParams['axes.unicode_minus'] = False

# 그래프 타입 명시적 지정
plt.bar(df['category'], df['values'])  # 막대그래프
plt.plot(df['x'], df['y'])  # 선그래프
```

### 3. 자주 사용한 디버깅 방법

```python
# 데이터 탐색
df.head()
df.describe()
df.value_counts()

# 결측치 확인
df.isnull().sum()

# 데이터 형태 확인
df.shape
df.columns.tolist()

# 그래프 전 데이터 확인
print(df.groupby('category').size())
```

## 배운 점

1. **데이터 로드 후 반드시 타입 확인하기** - `df.dtypes`와 `df.info()` 습관화
2. **그래프 그리기 전 데이터 전처리 완료하기** - 타입 변환, 정렬, 결측치 처리
3. **에러 메시지 꼼꼼히 읽기** - 대부분 데이터 타입 관련 힌트가 있음
4. **작은 단위로 테스트하기** - 전체 데이터 말고 샘플로 먼저 확인

## 다음에 주의할 점

- CSV 읽을 때 `dtype` 파라미터 활용하기
- 날짜 데이터는 `parse_dates` 옵션 사용하기
- 카테고리형 데이터는 명시적으로 순서 지정하기
- 그래프 스타일링은 데이터 확인 후에 하기

---
*오늘도 데이터와 한 판 승부... 이겼다! 🏆*