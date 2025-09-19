# Django Templates System & Django Template Language 완전 정리 TIL

## Django Templates System 개요

Django의 템플릿 시스템은 **프레젠테이션 계층과 비즈니스 로직을 완전히 분리**하기 위해 설계된 강력한 도구이다. 이는 MVC(Model-View-Controller) 패턴에서 View에 해당하는 부분을 담당하며, 웹 개발에서 HTML, XML, 이메일 등 다양한 텍스트 기반 형식을 동적으로 생성할 수 있게 해준다.

### Django 템플릿 시스템의 핵심 철학

1. **보안 우선**: 기본적으로 모든 변수는 HTML 이스케이핑된다
2. **단순함**: 복잡한 Python 코드를 템플릿에서 실행할 수 없다
3. **재사용성**: 템플릿 상속과 inclusion을 통한 코드 재활용
4. **확장성**: 커스텀 태그와 필터를 통한 기능 확장

### 주요 특징

- **자동 HTML 이스케이핑**: XSS(Cross-Site Scripting) 공격 방지
- **템플릿 상속 시스템**: 코드 중복 최소화 및 일관된 레이아웃 유지
- **컨텍스트 프로세서**: 글로벌 변수를 모든 템플릿에서 사용 가능
- **국제화(i18n) 지원**: 다국어 웹사이트 개발 지원
- **캐싱 지원**: 성능 최적화를 위한 템플릿 캐싱
- **디버깅 도구**: 개발 중 템플릿 오류 추적 및 디버깅

## Django Template Language (DTL) 상세 문법

Django Template Language는 템플릿 내에서 사용하는 특별한 구문 체계이다. Python과 유사하지만 더 제한적이고 안전하게 설계되었다.

### 1. 변수 (Variables)

변수는 이중 중괄호 `{{ }}`로 감싸서 표현한다.

```django
<!-- 기본 변수 출력 -->
{{ user.username }}
{{ product.name }}
{{ settings.SITE_NAME }}

<!-- 딕셔너리 접근 -->
{{ user.profile.bio }}
{{ data.user_info.email }}

<!-- 리스트/튜플 인덱스 접근 -->
{{ items.0 }}        <!-- 첫 번째 요소 -->
{{ categories.1 }}   <!-- 두 번째 요소 -->
{{ data.-1 }}        <!-- 마지막 요소 -->

<!-- 메서드 호출 (인수 없는 경우만 가능) -->
{{ user.get_full_name }}
{{ post.get_absolute_url }}

<!-- 변수가 존재하지 않는 경우 -->
{{ nonexistent_variable }}  <!-- 빈 문자열로 출력 -->
```

### 2. 태그 (Tags)

태그는 `{% %}`로 감싸며 템플릿의 논리적 구조를 제어한다.

#### 조건문 태그

```django
<!-- 기본 if 문 -->
{% if user.is_authenticated %}
    <p>환영한다, {{ user.username }}님!</p>
{% endif %}

<!-- if-else 문 -->
{% if user.is_staff %}
    <p>관리자 메뉴</p>
{% else %}
    <p>일반 사용자 메뉴</p>
{% endif %}

<!-- if-elif-else 문 -->
{% if user.is_superuser %}
    <p>최고 관리자</p>
{% elif user.is_staff %}
    <p>스태프</p>
{% elif user.is_authenticated %}
    <p>인증된 사용자</p>
{% else %}
    <p>익명 사용자</p>
{% endif %}

<!-- 논리 연산자 사용 -->
{% if user.is_authenticated and user.is_active %}
    <p>활성화된 사용자</p>
{% endif %}

{% if user.is_staff or user.is_superuser %}
    <p>관리 권한 보유</p>
{% endif %}

{% if not user.is_anonymous %}
    <p>로그인한 사용자</p>
{% endif %}

<!-- 비교 연산자 -->
{% if user.age >= 18 %}
    <p>성인 사용자</p>
{% endif %}

{% if posts.count == 0 %}
    <p>게시물이 없다</p>
{% endif %}

<!-- in 연산자 -->
{% if 'admin' in user.groups.all %}
    <p>관리자 그룹 소속</p>
{% endif %}
```

#### 반복문 태그

```django
<!-- 기본 for 문 -->
{% for post in posts %}
    <article>
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
    </article>
{% endfor %}

<!-- empty 절 사용 -->
{% for product in products %}
    <div class="product">
        <h3>{{ product.name }}</h3>
        <p>가격: {{ product.price }}원</p>
    </div>
{% empty %}
    <p>상품이 없다.</p>
{% endfor %}

<!-- forloop 변수 활용 -->
{% for item in items %}
    <div class="item-{{ forloop.counter }}">
        <p>{{ forloop.counter }}. {{ item.name }}</p>
        
        {% if forloop.first %}
            <span class="first">첫 번째</span>
        {% endif %}
        
        {% if forloop.last %}
            <span class="last">마지막</span>
        {% endif %}
        
        <small>전체 {{ forloop.revcounter }}개 중 {{ forloop.counter0 }}번째 (0부터 시작)</small>
    </div>
{% endfor %}

<!-- 중첩 반복문에서 외부 루프 접근 -->
{% for category in categories %}
    <h2>{{ category.name }}</h2>
    {% for product in category.products.all %}
        <p>{{ forloop.parentloop.counter }}.{{ forloop.counter }} {{ product.name }}</p>
    {% endfor %}
{% endfor %}

<!-- 딕셔너리 순회 -->
{% for key, value in data.items %}
    <p>{{ key }}: {{ value }}</p>
{% endfor %}
```

#### 템플릿 상속 관련 태그

```django
<!-- base.html -->
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}기본 제목{% endblock %}</title>
    
    {% block extra_css %}{% endblock %}
    
    <link rel="stylesheet" href="{% static 'css/base.css' %}">
</head>
<body>
    <header>
        {% block header %}
            <nav>
                <ul>
                    <li><a href="{% url 'home' %}">홈</a></li>
                    <li><a href="{% url 'about' %}">소개</a></li>
                </ul>
            </nav>
        {% endblock %}
    </header>
    
    <main>
        {% block content %}
            <!-- 기본 내용 -->
        {% endblock %}
    </main>
    
    <footer>
        {% block footer %}
            <p>&copy; 2024 우리 사이트</p>
        {% endblock %}
    </footer>
    
    {% block extra_js %}{% endblock %}
</body>
</html>

<!-- child.html -->
{% extends "base.html" %}
{% load static %}

{% block title %}홈페이지 - {{ block.super }}{% endblock %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'css/home.css' %}">
{% endblock %}

{% block content %}
    <h1>메인 페이지</h1>
    <section class="hero">
        <h2>환영한다!</h2>
        <p>우리 웹사이트에 오신 것을 환영한다.</p>
    </section>
    
    <section class="features">
        {% for feature in features %}
            <div class="feature">
                <h3>{{ feature.title }}</h3>
                <p>{{ feature.description }}</p>
            </div>
        {% endfor %}
    </section>
{% endblock %}

{% block extra_js %}
    <script src="{% static 'js/home.js' %}"></script>
{% endblock %}
```

#### 템플릿 포함 태그

```django
<!-- 기본 include -->
{% include "partials/header.html" %}

<!-- 컨텍스트와 함께 include -->
{% include "partials/product_card.html" with product=featured_product %}

<!-- 컨텍스트만 전달 (부모 컨텍스트 차단) -->
{% include "partials/sidebar.html" with user=current_user only %}

<!-- 조건부 include -->
{% if user.is_authenticated %}
    {% include "partials/user_menu.html" %}
{% else %}
    {% include "partials/guest_menu.html" %}
{% endif %}

<!-- 동적 템플릿 이름 -->
{% include template_name %}
```

### 3. 필터 (Filters)

필터는 파이프 `|` 기호를 사용하여 변수의 값을 변환하거나 형식을 지정한다.

#### 문자열 필터

```django
<!-- 대소문자 변환 -->
{{ name|upper }}           <!-- 모두 대문자 -->
{{ name|lower }}           <!-- 모두 소문자 -->
{{ name|title }}           <!-- 각 단어의 첫 글자만 대문자 -->
{{ name|capfirst }}        <!-- 첫 글자만 대문자 -->

<!-- 문자열 조작 -->
{{ content|truncatewords:50 }}      <!-- 50단어로 자르기 -->
{{ content|truncatechars:100 }}     <!-- 100문자로 자르기 -->
{{ text|slice:":10" }}              <!-- 처음 10문자만 -->
{{ text|slice:"10:" }}              <!-- 10번째 문자부터 끝까지 -->

<!-- HTML 관련 -->
{{ content|striptags }}             <!-- HTML 태그 제거 -->
{{ content|safe }}                  <!-- HTML 이스케이핑 해제 -->
{{ content|escape }}                <!-- HTML 이스케이핑 강제 -->
{{ text|linebreaks }}               <!-- 줄바꿈을 <p>, <br> 태그로 변환 -->
{{ text|linebreaksbr }}             <!-- 줄바꿈을 <br> 태그로만 변환 -->

<!-- URL 관련 -->
{{ url|urlize }}                    <!-- URL을 링크로 변환 -->
{{ text|urlizetrunc:30 }}           <!-- URL을 30문자로 자른 링크로 변환 -->
```

#### 숫자 및 날짜 필터

```django
<!-- 숫자 포맷 -->
{{ price|floatformat }}             <!-- 소수점 불필요시 제거 -->
{{ price|floatformat:2 }}           <!-- 소수점 2자리까지 -->
{{ number|add:10 }}                 <!-- 10 더하기 -->
{{ value|divisibleby:3 }}           <!-- 3으로 나누어 떨어지는지 -->

<!-- 날짜 포맷 -->
{{ post.created|date:"Y-m-d" }}           <!-- 2024-03-15 -->
{{ post.created|date:"Y년 m월 d일" }}      <!-- 2024년 03월 15일 -->
{{ post.created|time:"H:i" }}             <!-- 14:30 -->
{{ post.created|timesince }}              <!-- "2시간 전" -->
{{ post.created|timeuntil }}              <!-- "3일 후" -->

<!-- 현재 시간 -->
{% now "Y-m-d H:i:s" %}                   <!-- 현재 시간 출력 -->
```

#### 리스트 및 딕셔너리 필터

```django
<!-- 리스트 조작 -->
{{ items|length }}              <!-- 길이 -->
{{ items|first }}               <!-- 첫 번째 요소 -->
{{ items|last }}                <!-- 마지막 요소 -->
{{ items|join:", " }}           <!-- 콤마로 결합 -->
{{ items|slice:":3" }}          <!-- 처음 3개 요소 -->
{{ items|random }}              <!-- 임의 요소 -->

<!-- 정렬 -->
{{ items|dictsort:"name" }}     <!-- name 필드로 정렬 -->
{{ items|dictsortreversed:"created" }}  <!-- created 필드로 역순 정렬 -->
```

#### 기타 유용한 필터

```django
<!-- 기본값 설정 -->
{{ name|default:"익명" }}              <!-- name이 없으면 "익명" -->
{{ name|default_if_none:"없음" }}      <!-- name이 None이면 "없음" -->

<!-- 파일 관련 -->
{{ file.name|filesizeformat }}         <!-- "1.2 MB" 형태로 파일 크기 -->

<!-- JSON -->
{{ data|pprint }}                      <!-- 보기 좋게 JSON 출력 -->

<!-- 필터 체이닝 -->
{{ content|striptags|truncatewords:30|title }}
```

### 4. 주석 (Comments)

```django
{# 한 줄 주석 #}

{% comment %}
여러 줄 주석
이 부분은 렌더링되지 않는다
{% endcomment %}

{% comment "TODO" %}
나중에 구현할 기능에 대한 메모
- 사용자 프로필 이미지 업로드
- 알림 기능
{% endcomment %}
```

## 템플릿 구조와 조직

### 권장 디렉토리 구조

```
myproject/
├── templates/
│   ├── base.html
│   ├── 404.html
│   ├── 500.html
│   ├── registration/
│   │   ├── login.html
│   │   └── signup.html
│   └── partials/
│       ├── header.html
│       ├── footer.html
│       └── pagination.html
├── myapp/
│   └── templates/
│       └── myapp/
│           ├── index.html
│           ├── detail.html
│           └── list.html
└── static/
    ├── css/
    ├── js/
    └── images/
```