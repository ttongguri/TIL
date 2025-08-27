```css
/* CSS: 스타일링에 집중 */
.site-header {
  background-color: #f8f9fa;
  padding: 1rem 0;
  border-bottom: 1px solid #dee2e6;
}

.site-title {
  color: #333;
  font-size: 2rem;
  margin: 0;
}

.main-navigation ul {
  list-style: none;
  display: flex;
  gap: 1rem;
  margin: 0;
  padding: 0;
}

.blog-post {
  max-width: 800px;
  margin: 2rem auto;
  padding: 0 1rem;
}

.post-header {
  border-bottom: 1px solid #eee;
  padding-bottom: 1rem;
  margin-bottom: 2rem;
}

.post-title {
  color: #222;
  font-size: 1.8rem;
  margin-bottom: 0.5rem;
}

.post-date {
  color: #666;
  font-style: italic;
}

.sidebar {
  background-color: #f8f9fa;
  padding: 1rem;
  border-radius: 8px;
}

.site-footer {
  background-color: #343a40;
  color: white;
  text-align: center;
  padding: 2rem 0;
  margin-top: 3rem;
}
```

---# HTML과 CSS 역할 분리 및 OOCSS 개념 정리

## HTML과 CSS의 역할 분리 원칙

### 기본 개념
- **HTML**: 문서의 구조와 의미를 담당 (Structure & Semantics)
- **CSS**: 시각적 표현과 스타일링을 담당 (Presentation)

### 시멘틱 마크업 (Semantic Markup)
시멘틱 마크업은 HTML 태그를 사용할 때 **의미**에 맞게 사용하는 것을 말합니다.

#### 시멘틱 마크업의 핵심
- 태그의 **시각적 결과**가 아닌 **의미**에 따라 선택
- 콘텐츠가 무엇인지, 어떤 역할을 하는지에 집중
- 브라우저, 검색엔진, 보조기술이 내용을 이해할 수 있도록 도움

#### 시멘틱 HTML5 태그들
```html
<header>    <!-- 헤더, 제목 영역 -->
<nav>       <!-- 내비게이션 메뉴 -->
<main>      <!-- 주요 콘텐츠 -->
<article>   <!-- 독립적인 글/콘텐츠 -->
<section>   <!-- 논리적 구획/섹션 -->
<aside>     <!-- 부차적 콘텐츠 (사이드바 등) -->
<footer>    <!-- 하단, 저작권 정보 등 -->
<figure>    <!-- 이미지, 도표와 캡션 -->
<figcaption> <!-- figure의 캡션 -->
<time>      <!-- 날짜/시간 -->
<mark>      <!-- 강조/하이라이트된 텍스트 -->
```

### 왜 분리해야 할까?
1. **유지보수성**: 구조와 스타일을 독립적으로 수정 가능
2. **재사용성**: 동일한 HTML 구조에 다양한 스타일 적용 가능
3. **접근성**: 스크린 리더 등이 구조를 명확히 파악 가능
4. **SEO**: 검색 엔진이 콘텐츠 구조를 쉽게 이해 가능
5. **시멘틱**: 기계와 인간 모두가 콘텐츠의 의미를 이해 가능

### 비시멘틱 vs 시멘틱 비교

#### 잘못된 예시 (비시멘틱) ❌
```html
<!-- HTML에서 스타일을 직접 지정 -->
<div style="color: red; font-size: 20px; margin: 10px;">
  제목
</div>

<!-- 의미 없는 태그 사용 -->
<div class="red-text big-font">제목</div>
<div class="container">
  <div class="top-section">내비게이션</div>
  <div class="main-content">
    <div class="post">
      <div class="post-title">글 제목</div>
      <div class="post-date">2024-08-27</div>
      <div class="post-text">본문...</div>
    </div>
  </div>
  <div class="right-sidebar">광고</div>
</div>
```

#### 올바른 예시 (시멘틱) ✅
```html
<!-- HTML: 의미적 구조에 집중 -->
<header class="site-header">
  <h1 class="site-title">사이트 제목</h1>
  <nav class="main-navigation">
    <ul>
      <li><a href="/">홈</a></li>
      <li><a href="/about">소개</a></li>
      <li><a href="/blog">블로그</a></li>
    </ul>
  </nav>
</header>

<main class="main-content">
  <article class="blog-post">
    <header class="post-header">
      <h2 class="post-title">블로그 글 제목</h2>
      <time class="post-date" datetime="2024-08-27">2024년 8월 27일</time>
    </header>
    <div class="post-content">
      <p>본문 내용...</p>
      <figure class="post-image">
        <img src="image.jpg" alt="이미지 설명">
        <figcaption>이미지 캡션</figcaption>
      </figure>
    </div>
    <footer class="post-footer">
      <p>작성자: <strong>홍길동</strong></p>
    </footer>
  </article>
</main>

<aside class="sidebar">
  <section class="widget">
    <h3>최근 글</h3>
    <ul>
      <li><a href="#">최근 글 1</a></li>
      <li><a href="#">최근 글 2</a></li>
    </ul>
  </section>
</aside>

<footer class="site-footer">
  <p>&copy; 2024 내 블로그. 모든 권리 보유.</p>
</footer>
```

### 시멘틱 마크업의 장점

1. **접근성 향상**: 스크린 리더가 구조를 정확히 파악
2. **SEO 개선**: 검색엔진이 콘텐츠의 중요도와 맥락을 이해
3. **유지보수성**: 코드의 의미가 명확해 수정이 쉬움
4. **호환성**: 다양한 기기와 브라우저에서 일관된 동작
5. **미래 지향적**: 새로운 기술과 표준에 대응하기 쉬움

### 시멘틱 마크업 실전 팁

#### 1. 제목 태그 올바르게 사용하기
```html
<!-- 잘못된 예시 ❌ -->
<div class="big-text">페이지 제목</div>
<div class="medium-text">섹션 제목</div>

<!-- 올바른 예시 ✅ -->
<h1>페이지 제목</h1>
<h2>섹션 제목</h2>
<h3>하위 섹션 제목</h3>
```

#### 2. 목록 구조 활용하기
```html
<!-- 잘못된 예시 ❌ -->
<div>메뉴1</div>
<div>메뉴2</div>
<div>메뉴3</div>

<!-- 올바른 예시 ✅ -->
<ul>
  <li>메뉴1</li>
  <li>메뉴2</li>
  <li>메뉴3</li>
</ul>
```

#### 3. 강조 표현하기
```html
<!-- 잘못된 예시 ❌ -->
<div class="bold">중요한 내용</div>
<div class="italic">강조하고 싶은 내용</div>

<!-- 올바른 예시 ✅ -->
<strong>중요한 내용</strong>      <!-- 의미상 중요함 -->
<em>강조하고 싶은 내용</em>       <!-- 의미상 강조 -->
<mark>하이라이트된 내용</mark>    <!-- 참조용 하이라이트 -->
```

---

## OOCSS (Object-Oriented CSS)

### OOCSS란?
Nicole Sullivan이 제안한 CSS 작성 방법론으로, 객체지향 프로그래밍의 개념을 CSS에 적용한 것

### OOCSS의 두 가지 핵심 원칙

#### 1. 구조와 외관의 분리 (Separate Structure and Skin)
객체의 구조(레이아웃)와 외관(색상, 폰트 등)을 분리

**잘못된 예시 ❌**
```css
.blue-button {
  padding: 10px 20px;
  border-radius: 5px;
  background-color: blue;
  color: white;
  border: none;
}

.red-button {
  padding: 10px 20px;  /* 중복 */
  border-radius: 5px;  /* 중복 */
  background-color: red;
  color: white;
  border: none;        /* 중복 */
}
```

**올바른 예시 ✅**
```css
/* 구조 (Structure) */
.btn {
  padding: 10px 20px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

/* 외관 (Skin) */
.btn-primary {
  background-color: blue;
  color: white;
}

.btn-danger {
  background-color: red;
  color: white;
}

.btn-success {
  background-color: green;
  color: white;
}
```

```html
<button class="btn btn-primary">확인</button>
<button class="btn btn-danger">삭제</button>
<button class="btn btn-success">저장</button>
```

#### 2. 컨테이너와 내용의 분리 (Separate Container and Content)
객체가 어떤 컨테이너에 있든 동일하게 보이도록 설계

**잘못된 예시 ❌**
```css
.sidebar h3 {
  font-size: 18px;
  color: #333;
  margin-bottom: 10px;
}

.content h3 {
  font-size: 18px;  /* 중복 */
  color: #333;      /* 중복 */
  margin-bottom: 10px; /* 중복 */
}
```

**올바른 예시 ✅**
```css
/* 재사용 가능한 제목 스타일 */
.heading-tertiary {
  font-size: 18px;
  color: #333;
  margin-bottom: 10px;
}

/* 컨테이너별 특별한 스타일이 필요한 경우만 */
.sidebar .heading-tertiary {
  border-left: 3px solid #007bff;
  padding-left: 10px;
}
```

```html
<aside class="sidebar">
  <h3 class="heading-tertiary">사이드바 제목</h3>
</aside>

<main class="content">
  <h3 class="heading-tertiary">메인 제목</h3>
</main>
```

### OOCSS의 실전 예시

#### 카드 컴포넌트 만들기
```css
/* 기본 카드 구조 */
.card {
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* 카드 외관 변형 */
.card-primary {
  background-color: #e3f2fd;
  border-left: 4px solid #2196f3;
}

.card-warning {
  background-color: #fff3e0;
  border-left: 4px solid #ff9800;
}

.card-success {
  background-color: #e8f5e8;
  border-left: 4px solid #4caf50;
}

/* 카드 크기 변형 */
.card-small {
  padding: 10px;
  font-size: 14px;
}

.card-large {
  padding: 30px;
  font-size: 18px;
}
```

```html
<div class="card card-primary">
  <h3>기본 정보 카드</h3>
  <p>일반적인 정보를 담고 있습니다.</p>
</div>

<div class="card card-warning card-small">
  <h3>주의사항</h3>
  <p>작은 크기의 경고 카드입니다.</p>
</div>

<div class="card card-success card-large">
  <h3>성공 메시지</h3>
  <p>큰 크기의 성공 카드입니다.</p>
</div>
```

### OOCSS의 장점
1. **코드 재사용성**: 동일한 스타일을 여러 곳에서 활용
2. **유지보수성**: 한 곳만 수정하면 전체에 반영
3. **CSS 파일 크기 감소**: 중복 코드 제거
4. **일관성**: 디자인 시스템 구축 가능
5. **확장성**: 새로운 변형을 쉽게 추가

### OOCSS의 단점
1. **클래스명 증가**: HTML에 여러 클래스가 필요
2. **학습 곡선**: 초기 설계가 중요하고 복잡할 수 있음
3. **과도한 추상화**: 너무 세분화하면 오히려 복잡해질 수 있음

### 실무 적용 팁
1. **재사용되는 스타일부터 시작**: 버튼, 카드, 폼 요소 등
2. **네이밍 컨벤션 정하기**: BEM, SMACSS 등과 함께 사용
3. **문서화**: 컴포넌트 가이드 작성
4. **점진적 적용**: 기존 프로젝트에 조금씩 도입

---

## 마무리
HTML과 CSS의 역할 분리와 OOCSS 방법론을 통해 더 유지보수하기 쉽고 확장 가능한 코드를 작성할 수 있습니다. 특히 팀 프로젝트나 장기간 운영되는 서비스에서 그 진가를 발휘합니다.