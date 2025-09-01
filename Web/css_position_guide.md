# CSS Position 속성 완벽 정리

## 개요
CSS의 `position` 속성은 요소가 문서 내에서 어떻게 배치될지를 결정한다. 5가지 주요 값이 있으며, 각각 다른 배치 방식을 제공한다.

## 1. static (기본값)
```css
position: static;
```

**특징:**
- 모든 요소의 기본값
- 문서의 일반적인 흐름(normal flow)을 따름
- `top`, `right`, `bottom`, `left`, `z-index` 속성이 적용되지 않음
- 다른 요소들과 순서대로 배치됨

**사용 예시:**
```css
.static-box {
    position: static;
    background-color: lightblue;
    padding: 20px;
}
```

## 2. relative
```css
position: relative;
```

**특징:**
- 원래 있어야 할 위치를 기준으로 상대적으로 이동
- 다른 요소들은 이 요소가 원래 위치에 있는 것처럼 취급함 (공간 유지)
- `top`, `right`, `bottom`, `left` 속성으로 위치 조정 가능
- `z-index` 적용 가능

**사용 예시:**
```css
.relative-box {
    position: relative;
    top: 20px;
    left: 30px;
    background-color: lightgreen;
}
```

## 3. absolute
```css
position: absolute;
```

**특징:**
- 문서의 일반적인 흐름에서 제거됨
- 가장 가까운 `position`이 설정된 조상 요소를 기준으로 배치됨
- 조상 중 `position`이 설정된 요소가 없으면 `<html>` 기준
- 다른 요소들이 이 요소의 공간을 차지함
- `top`, `right`, `bottom`, `left`로 정확한 위치 지정

**사용 예시:**
```css
.parent {
    position: relative;
    width: 300px;
    height: 200px;
}

.absolute-box {
    position: absolute;
    top: 10px;
    right: 10px;
    background-color: coral;
}
```

## 4. fixed
```css
position: fixed;
```

**특징:**
- 뷰포트(브라우저 창)를 기준으로 고정됨
- 스크롤해도 위치가 변하지 않음
- 문서의 일반적인 흐름에서 제거됨
- 항상 화면의 같은 위치에 표시됨

**사용 예시:**
```css
.fixed-header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background-color: navy;
    color: white;
    z-index: 1000;
}
```

## 5. sticky
```css
position: sticky;
```

**특징:**
- `relative`와 `fixed`의 하이브리드
- 스크롤 위치에 따라 동작이 바뀜
- 설정된 임계값까지는 `relative`처럼 동작함
- 임계값에 도달하면 `fixed`처럼 고정됨
- 부모 요소를 벗어나면 다시 일반 흐름을 따름

**사용 예시:**
```css
.sticky-nav {
    position: sticky;
    top: 0;
    background-color: white;
    border-bottom: 1px solid #ccc;
    z-index: 100;
}
```

## 위치 조정 속성

### top, right, bottom, left
```css
.positioned-element {
    position: absolute;
    top: 10px;    /* 위쪽에서 10px */
    right: 20px;  /* 오른쪽에서 20px */
    bottom: 30px; /* 아래쪽에서 30px */
    left: 40px;   /* 왼쪽에서 40px */
}
```

### z-index
```css
.front-element {
    position: relative;
    z-index: 10;  /* 숫자가 클수록 앞에 표시 */
}
```

## 실전 활용 예시

### 1. 모달 창
```css
.modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1000;
}
```

### 2. 네비게이션 바
```css
.navbar {
    position: sticky;
    top: 0;
    background: white;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
```

### 3. 툴팁
```css
.tooltip {
    position: absolute;
    bottom: 100%;
    left: 50%;
    transform: translateX(-50%);
}
```

### 4. 사이드바 고정
```css
.sidebar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 250px;
}
```

## 주의사항

1. **absolute/fixed 요소의 크기**: 기본적으로 내용에 맞춰짐
2. **z-index**: `position`이 설정된 요소에만 적용됨
3. **sticky 브라우저 지원**: IE에서는 지원하지 않음
4. **성능**: `fixed`나 `sticky`는 리페인트 비용이 클 수 있음

## 요약 표

| Position | 기준점 | 공간 차지 | 스크롤 영향 | 주요 용도 |
|----------|--------|----------|------------|-----------|
| static | - | O | O | 기본 배치 |
| relative | 원래 위치 | O | O | 미세 조정 |
| absolute | 부모 요소 | X | O | 정확한 위치 |
| fixed | 뷰포트 | X | X | 고정 UI |
| sticky | 부모/뷰포트 | O | 조건부 | 스크롤 네비 |