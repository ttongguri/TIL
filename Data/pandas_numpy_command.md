# 🐼 **Pandas / NumPy 기본 문법 정리**

---

## 🐼 Pandas 기본

### 🔹 DataFrame 생성

```python
import pandas as pd
import numpy as np

# 디한셔너리로 생성
df = pd.DataFrame({
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 35],
    "Score": [85.5, 90.3, 78.9]
})

# 리스트 + columns 

df2 = pd.DataFrame(
    [["Alice", 25, 85.5], ["Bob", 30, 90.3], ["Charlie", 35, 78.9]],
    columns=["Name", "Age", "Score"]
)

# NumPy 배열로 생성
arr = np.array([[25, 85.5], [30, 90.3], [35, 78.9]])
df3 = pd.DataFrame(arr, columns=["Age", "Score"])
df3.insert(0, "Name", ["Alice", "Bob", "Charlie"])
```

---

### 🔹 정보 확인

```python
df.shape        # (행 개수, 열 개수)
df.columns      # 컬럼명 목록
df.info()       # 컬럼 타입, 결심치 정보
df.describe()   # 통계 요약
df.head(3)      # 상위 3개 행
df.tail(3)      # 하위 3개 행
```

---

### 🔹 인덱싱 / 슬라이싱

```python
df["Name"]              # 특정 컬럼
df[["Name", "Score"]]   # 여러 컬럼
df.iloc[0]              # 0번째 행
df.loc[1, "Age"]        # 인덱스 1의 Age 값
df.loc[df["Age"] > 30]  # 조건 필터링
```

---

### 🔹 기본 통계 연산

```python
df["Score"].mean()   # 평균
df["Score"].std()    # 표준편차
df["Score"].min()    # 최소값
df["Score"].max()    # 최대값
df["Score"].sum()    # 합계
df[df["Age"] > 25]["Score"].mean()  # 조건 평균
```

---

### 🔹 통계 / 값 검색

```python
df["Age"].nunique()         # 고유값 개수 (class 개수)
df["Age"].value_counts()    # 각 값 별 개수
df["Age"].value_counts(normalize=True) * 100  # 비율(%)
df.corr(numeric_only=True)  # 컬럼 간 상관관계
df.groupby("Class")["Score"].mean()  # 그룹별 평균
```

---

### 🔹 기타 기능

```python
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")   # 문자열 → 숫자
df["Age"].fillna(df["Age"].mean(), inplace=True)        # 결심치 처리
df.sort_values("Score", ascending=False)                # 정렬
df.reset_index(drop=True, inplace=True)                 # 인덱스 리셋
```

---

## 🔢 NumPy 기본

### 🔹 배열 생성

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([[1, 2, 3], [4, 5, 6]])

np.zeros((2, 3))     # 0
np.ones((2, 3))      # 1
np.eye(3)            # 단위행렬
np.arange(0, 10, 2)  # [0,2,4,6,8]
np.linspace(0, 2*np.pi, 100)  # 0~2π 구간 100분
```

---

### 🔹 배열 정보

```python
a.shape     # (3,)
b.ndim      # 차원 수
a.dtype     # 타입
```

---

### 🔹 기본 연산

```python
a + 10      # [11,12,13]
a * 2       # [2,4,6]
a ** 2      # [1,4,9]
a.sum()     # 6
a.mean()    # 2
a.std()     # 0.816
a.min(), a.max()
```

---

### 🔹 인덱싱 / 슬라이싱

```python
a[0]        # 1
b[1, 2]     # 6
b[:, 1]     # [2,5]
b[0:2, 0:2] # [[1,2],[4,5]]
```

---

### 🔹 조건 / 불리언 마스크

```python
a[a > 1]       # [2,3]
b[b % 2 == 0]  # 짝수만 추출
```

---

### 🔹 행렬 연산

```python
A = np.array([[1,2],[3,4]])
B = np.array([[2,0],[1,3]])

A.dot(B)     # 행렬곱
np.linalg.inv(A)  # 역행렬
np.linalg.det(A)  # 행렬식
```

---

### 🔹 난수 생성

```python
np.random.seed(42)
np.random.rand(3, 2)         # [0,1) 구류분포
np.random.randn(3, 2)        # 표준정규분포
np.random.randint(0, 10, 5)  # 정수 난수
```

---

### 🔹 브로드캐스팅

```python
A = np.array([[1,2,3],[4,5,6]])
b = np.array([10,20,30])
A + b
```

결과:
```
[[11,22,33],
 [14,25,36]]
```

---

## 🔹 요약

| 구분 | 함수 | 설명 |
|------|------------|------|
| **Pandas** | `pd.DataFrame`, `.loc[]`, `.groupby()`, `.value_counts()` | 데이터 형태, 그룹화 |
| **통계/요약** | `.mean()`, `.std()`, `.min()`, `.max()`, `.corr()` | 기본 통계학 |
| **조건/필터** | `df[df["col"] > 10]` | 조건 검색 |
| **NumPy** | `np.array`, `np.arange`, `np.linspace` | 배열 생성 |
| **연산** | `+`, `*`, `.dot()`, `.sum()` | 수학과 행렬 |
| **불리언 마스크** | `[row, col]`, `조건식` | 데이터 선택 |
| **난수** | `np.random.*` | 시뮬