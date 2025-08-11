# 문자열 검색 알고리즘: KMP와 Boyer-Moore

## KMP 알고리즘 (Knuth-Morris-Pratt Algorithm)

### 핵심 개념

KMP 알고리즘은 문자열에서 패턴을 효율적으로 찾는 알고리즘입니다. 불일치가 발생했을 때 이미 비교한 정보를 활용하여 패턴을 최대한 적게 이동시키는 것이 핵심입니다.

### 작동 원리

1. **전처리 단계**: 패턴의 접두사와 접미사가 일치하는 최대 길이를 저장하는 실패 함수(failure function) 테이블을 생성
2. **검색 단계**: 불일치 발생 시 실패 함수를 참조하여 패턴을 적절히 이동

### 실패 함수 (Failure Function)

```
패턴: "ABCABCAB"
인덱스: 0 1 2 3 4 5 6 7
실패함수: 0 0 0 1 2 3 4 0
```

- 인덱스 3: "ABC"의 접두사와 접미사 중 일치하는 최대 길이는 0
- 인덱스 6: "ABCABCA"에서 "ABCA"가 접두사와 접미사로 일치 → 길이 4

### 시간 복잡도
- 전처리: O(m) - m은 패턴의 길이
- 검색: O(n) - n은 텍스트의 길이
- **전체: O(n + m)**

### 구현 예시

```python
def compute_lps(pattern):
    """실패 함수(LPS) 배열 계산"""
    m = len(pattern)
    lps = [0] * m
    length = 0
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    """KMP 알고리즘을 이용한 문자열 검색"""
    n, m = len(text), len(pattern)
    lps = compute_lps(pattern)
    
    i = j = 0
    results = []
    
    while i < n:
        if pattern[j] == text[i]:
            i += 1
            j += 1
            
        if j == m:
            results.append(i - j)
            j = lps[j - 1]
        elif i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
                
    return results
```

---

## Boyer-Moore 알고리즘

### 핵심 개념

Boyer-Moore 알고리즘은 패턴을 오른쪽에서 왼쪽으로 비교하면서, 불일치 발생 시 두 가지 휴리스틱을 사용하여 패턴을 크게 이동시키는 알고리즘입니다.

### 두 가지 휴리스틱

#### 1. Bad Character Heuristic (나쁜 문자 휴리스틱)
- 불일치 문자가 패턴에 없거나 현재 위치보다 왼쪽에 있을 때 패턴을 이동
- 각 문자의 패턴 내 마지막 출현 위치를 사전에 계산

#### 2. Good Suffix Heuristic (좋은 접미사 휴리스틱)
- 이미 일치한 접미사를 활용하여 패턴을 안전하게 이동
- 일치한 접미사가 패턴의 다른 위치에서도 나타나는 경우를 고려

### 시간 복잡도
- 최선의 경우: O(n/m) - 매번 패턴 길이만큼 건너뛸 수 있을 때
- 최악의 경우: O(nm) - 하지만 실제로는 매우 드물음
- **평균적으로 매우 빠름** (특히 긴 패턴에서)

### 구현 예시

```python
def bad_char_heuristic(pattern):
    """나쁜 문자 휴리스틱을 위한 테이블 생성"""
    bad_char = {}
    m = len(pattern)
    
    for i in range(m):
        bad_char[pattern[i]] = i
    
    return bad_char

def boyer_moore_search(text, pattern):
    """Boyer-Moore 알고리즘 (단순화된 버전 - Bad Character만)"""
    n, m = len(text), len(pattern)
    bad_char = bad_char_heuristic(pattern)
    results = []
    
    shift = 0
    while shift <= n - m:
        j = m - 1
        
        # 패턴을 오른쪽부터 비교
        while j >= 0 and pattern[j] == text[shift + j]:
            j -= 1
            
        if j < 0:  # 패턴 발견
            results.append(shift)
            # 다음 문자를 확인하여 shift 계산
            shift += (m - bad_char.get(text[shift + m], -1) - 1) if shift + m < n else 1
        else:
            # Bad character heuristic 적용
            shift += max(1, j - bad_char.get(text[shift + j], -1))
    
    return results
```

---

## 비교 분석

| 특징 | KMP | Boyer-Moore |
|------|-----|-------------|
| **비교 방향** | 왼쪽 → 오른쪽 | 오른쪽 → 왼쪽 |
| **시간 복잡도** | O(n + m) 보장 | 평균 O(n/m), 최악 O(nm) |
| **전처리 시간** | O(m) | O(m + σ) (σ: 알파벳 크기) |
| **공간 복잡도** | O(m) | O(m + σ) |
| **장점** | 안정적인 성능 | 긴 패턴에서 매우 빠름 |
| **단점** | 항상 모든 문자 확인 | 짧은 패턴에서는 오버헤드 |

---

## 실제 활용 예시

### KMP 알고리즘 활용
1. **텍스트 에디터의 검색 기능**
   - 대용량 문서에서 안정적인 검색 성능 보장
   
2. **바이러스 검사**
   - 파일에서 특정 바이러스 시그니처 패턴 검색
   
3. **DNA 시퀀스 분석**
   - 유전자 서열에서 특정 패턴 찾기

### Boyer-Moore 알고리즘 활용
1. **Unix grep 명령어**
   - 파일에서 패턴 검색할 때 사용 (특히 긴 패턴)
   
2. **컴파일러의 문자열 최적화**
   - 소스 코드에서 특정 키워드나 패턴 검색
   
3. **데이터베이스 검색**
   - 텍스트 필드에서 빠른 문자열 검색

---

## 언제 어떤 알고리즘을 사용할까?

### KMP를 선택하는 경우
- 짧은 패턴 (길이 < 10)
- 안정적인 성능이 중요한 경우
- 메모리 사용량을 최소화해야 하는 경우
- 최악의 경우에도 일정한 성능을 보장해야 하는 경우

### Boyer-Moore를 선택하는 경우
- 긴 패턴 (길이 > 10)
- 평균적으로 빠른 검색이 필요한 경우
- 큰 텍스트에서 상대적으로 적은 매칭이 예상되는 경우
- 일반적인 텍스트 검색 (자연어 처리)

---

## 심화 학습 포인트

### KMP 고급 활용
- **Z-알고리즘과의 관계**: Z-배열을 이용한 KMP 구현
- **문자열 압축**: 반복 패턴 찾기
- **회전 문자열 판별**: 두 문자열이 회전 관계인지 확인

### Boyer-Moore 확장
- **Sunday 알고리즘**: Boyer-Moore의 개선된 버전
- **Turbo Boyer-Moore**: 좋은 접미사 휴리스틱 최적화
- **다중 패턴 검색**: Aho-Corasick과의 결합

---

## 구현 시 주의사항

1. **인덱스 관리**: 특히 Boyer-Moore에서 역방향 비교 시 주의
2. **경계 조건**: 패턴이나 텍스트가 빈 문자열인 경우 처리
3. **유니코드 처리**: 다국어 문자 처리 시 문자 인코딩 고려
4. **메모리 효율성**: 대용량 텍스트 처리 시 메모리 사용량 최적화

## 마무리

두 알고리즘 모두 문자열 처리의 핵심 기법으로, 상황에 따라 적절히 선택하여 사용하는 것이 중요합니다. KMP는 안정성을, Boyer-Moore는 속도를 추구할 때 각각 유리합니다.