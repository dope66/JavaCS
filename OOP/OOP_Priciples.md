# 객체지향 프로그래밍(OOP) - 4대 원칙
OOP는 유지보수성과 확장성을 고려한 설계 철학. 다음은 OOP의 핵심 4가지 특징

## 목차
- [캡슐화 (Encapsulation)](#캡슐화-encapsulation)
- [상속 (Inheritance)](#상속-inheritance)
- [다형성 (Polymorphism)](#다형성-polymorphism)
- [추상화 (Abstraction)](#추상화-abstraction)

# 캡슐화 (Encapsulation)
## 1. 정의
- 객체의 필드(데이터)와 메서드를 하나의 단위로 묶고, 외부에서 직접 접근하지 못하게 차단하는 것
- 객체의 내부 상태를 보호하고 무결성을 유지

## 2. 왜 사용하는가?
- 외부로부터의 잘못된 접근 차단
- 변경에 유연한 구조 설계
- 보안성 및 유지보수성 향상

## 3. 핵심 키워드
- 접근 제어자 (private, protected, public)
- 정보 은닉 (Information Hiding)
- 인터페이스 최소화

## 4. 기본 예제
```java
public class Account {
    private int balance;
    
    public void deposit(int amount) {
        if (amount > 0) balance += amount;
    }
    
    public int getBalance() {
        return balance;
    }
}
```

# 상속 (Inheritance)
## 1. 정의
- 기존 클래스의 속성과 메서드를 새로운 클래스가 물려받아 사용하는 것
- 부모 클래스(상위 클래스)의 특성을 자식 클래스(하위 클래스)가 확장하거나 재사용

## 2. 왜 사용하는가?
- 코드의 재사용성 향상
- 중복 코드 제거
- 계층적 분류를 통한 체계적인 설계
- 유지보수 효율성 증대

## 3. 핵심 키워드
- 부모 클래스 (Parent Class, Super Class)
- 자식 클래스 (Child Class, Sub Class)
- 메서드 오버라이딩 (Method Overriding)
- IS-A 관계

## 4. 기본 예제
```java
// 부모 클래스
public class Animal {
    protected String name;
    
    public void eat() {
        System.out.println(name + "이(가) 먹습니다.");
    }
}

// 자식 클래스
public class Dog extends Animal {
    public Dog(String name) {
        this.name = name;
    }
    
    public void bark() {
        System.out.println(name + "이(가) 짖습니다.");
    }
}
```

# 다형성 (Polymorphism)
## 1. 정의
- 하나의 인터페이스나 메서드가 여러 형태로 구현되어 다양하게 동작할 수 있는 특성
- 같은 메시지에 대해 객체마다 다른 방식으로 응답하는 능력

## 2. 왜 사용하는가?
- 코드의 유연성과 확장성 향상
- 객체 간의 결합도 감소
- 런타임에 적절한 메서드 선택 가능
- 새로운 타입 추가 시 기존 코드 수정 최소화

## 3. 핵심 키워드
- 메서드 오버로딩 (Method Overloading)
- 메서드 오버라이딩 (Method Overriding)
- 동적 바인딩 (Dynamic Binding)
- 인터페이스와 구현체

## 4. 기본 예제
```java
// 인터페이스
interface Shape {
    double calculateArea();
}

// 구현체들
class Circle implements Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle implements Shape {
    private double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}

// 사용 예시
Shape[] shapes = {new Circle(5), new Rectangle(3, 4)};
for (Shape shape : shapes) {
    System.out.println("면적: " + shape.calculateArea());
}
```

# 추상화 (Abstraction)
## 1. 정의
- 복잡한 시스템에서 핵심적인 개념이나 기능에 집중하기 위해 불필요한 세부사항을 숨기는 것
- 공통된 특성과 기능을 추출하여 일반화하는 과정

## 2. 왜 사용하는가?
- 복잡성 관리 및 이해도 향상
- 설계의 일관성 유지
- 변경사항의 영향 범위 최소화
- 인터페이스와 구현의 분리

## 3. 핵심 키워드
- 추상 클래스 (Abstract Class)
- 인터페이스 (Interface)
- 추상 메서드 (Abstract Method)
- 구현 강제성

## 4. 기본 예제
```java
// 추상 클래스
abstract class Vehicle {
    protected String brand;
    
    public Vehicle(String brand) {
        this.brand = brand;
    }
    
    // 추상 메서드 - 하위 클래스에서 반드시 구현
    public abstract void start();
    
    // 일반 메서드
    public void stop() {
        System.out.println(brand + " 차량이 멈춥니다.");
    }
}

// 구현 클래스
class Car extends Vehicle {
    public Car(String brand) {
        super(brand);
    }
    
    @Override
    public void start() {
        System.out.println(brand + " 자동차가 시동을 겁니다.");
    }
}

class Motorcycle extends Vehicle {
    public Motorcycle(String brand) {
        super(brand);
    }
    
    @Override
    public void start() {
        System.out.println(brand + " 오토바이가 시동을 겁니다.");
    }
}
```
