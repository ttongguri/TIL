# Python 객체지향 프로그래밍 - MRO, 상속, super(), 오버라이딩

## 상속 (Inheritance)

### 기본 개념
- 기존 클래스의 속성과 메서드를 새로운 클래스가 물려받는 것
- 코드 재사용성과 확장성을 높임
- `class 자식클래스(부모클래스):` 형태로 선언

### 예시 코드
```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        print(f"{self.name}이(가) 소리를 냅니다")

class Dog(Animal):  # Animal 클래스 상속
    def __init__(self, name, breed):
        super().__init__(name)  # 부모 클래스 초기화
        self.breed = breed
    
    def speak(self):  # 메서드 오버라이딩
        print(f"{self.name}이(가) 멍멍 짖습니다")
```

## 오버라이딩 (Overriding)

### 기본 개념
- 부모 클래스에서 정의된 메서드를 자식 클래스에서 재정의하는 것
- 같은 이름의 메서드로 다른 동작을 구현
- 다형성(Polymorphism)의 핵심 개념

### 예시 코드
```python
class Shape:
    def area(self):
        return 0

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):  # 오버라이딩
        return 3.14 * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):  # 오버라이딩
        return self.width * self.height
```

## super() 함수

### 기본 개념
- 부모 클래스의 메서드나 속성에 접근할 때 사용
- 다중 상속에서 MRO를 따라 메서드를 호출
- 명시적으로 부모 클래스를 지정하지 않아도 됨

### 사용법
```python
class Parent:
    def __init__(self, name):
        self.name = name
        print(f"Parent 초기화: {name}")
    
    def greet(self):
        print(f"안녕하세요, {self.name}입니다")

class Child(Parent):
    def __init__(self, name, age):
        super().__init__(name)  # 부모 클래스 초기화 호출
        self.age = age
        print(f"Child 초기화: {age}세")
    
    def greet(self):
        super().greet()  # 부모 클래스 메서드 호출
        print(f"저는 {self.age}세입니다")
```

## MRO (Method Resolution Order)

### 기본 개념
- 메서드 탐색 순서를 정의하는 규칙
- 다중 상속에서 어떤 부모 클래스의 메서드를 먼저 호출할지 결정
- C3 선형화 알고리즘 사용
- `클래스명.__mro__` 또는 `클래스명.mro()`로 확인 가능

### 다중 상속 예시
```python
class A:
    def method(self):
        print("A의 메서드")

class B(A):
    def method(self):
        print("B의 메서드")
        super().method()

class C(A):
    def method(self):
        print("C의 메서드")
        super().method()

class D(B, C):
    def method(self):
        print("D의 메서드")
        super().method()

# MRO 확인
print(D.__mro__)
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

# 메서드 호출
d = D()
d.method()
# 출력:
# D의 메서드
# B의 메서드
# C의 메서드
# A의 메서드
```

### MRO 규칙
1. 자식 클래스가 부모 클래스보다 먼저
2. 부모 클래스들은 상속 순서대로
3. 공통 부모는 가장 마지막에
4. 일관성 유지 (linearization)

## 실전 활용 예시

```python
class Vehicle:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def start(self):
        print(f"{self.brand} {self.model} 시동을 걸었습니다")
    
    def info(self):
        print(f"브랜드: {self.brand}, 모델: {self.model}")

class Electric:
    def __init__(self, battery_capacity):
        self.battery_capacity = battery_capacity
    
    def charge(self):
        print(f"배터리 {self.battery_capacity}kWh 충전 중...")

class Car(Vehicle):
    def __init__(self, brand, model, doors):
        super().__init__(brand, model)
        self.doors = doors
    
    def info(self):
        super().info()
        print(f"문 개수: {self.doors}")

class ElectricCar(Car, Electric):
    def __init__(self, brand, model, doors, battery_capacity):
        Car.__init__(self, brand, model, doors)
        Electric.__init__(self, battery_capacity)
    
    def start(self):
        print("전기차 조용히 시동...")
        super(Car, self).start()  # Vehicle의 start 호출
    
    def info(self):
        super().info()
        print(f"배터리 용량: {self.battery_capacity}kWh")

# 사용 예시
tesla = ElectricCar("Tesla", "Model 3", 4, 75)
tesla.info()
tesla.start()
tesla.charge()

# MRO 확인
print(ElectricCar.__mro__)
```

## 핵심 포인트

### 상속 사용 시 주의사항
- IS-A 관계일 때 사용 (상속 vs 구성)
- 너무 깊은 상속 계층 피하기
- 리스코프 치환 원칙 고려

### super() 사용 팁
- `__init__`에서 부모 클래스 초기화 필수
- 다중 상속에서는 모든 부모 클래스 초기화 고려
- MRO를 이해하고 사용하기

### 오버라이딩 베스트 프랙티스
- 부모 클래스의 인터페이스 유지
- 필요에 따라 super() 호출로 부모 기능 확장
- 문서화로 변경사항 명시

### MRO 디버깅
```python
# MRO 시각화
def print_mro(cls):
    print(f"{cls.__name__}의 MRO:")
    for i, klass in enumerate(cls.__mro__):
        print(f"  {i+1}. {klass.__name__}")

print_mro(ElectricCar)
```