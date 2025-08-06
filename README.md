# CS

### 1. OOP의 4가지 특징


OOP의 4가지 특징 캡슐화, 상속, 다형성, 추상화

**캡슐화**는 데이터와 그 데이터를 처리하는 메소드를 하나로 묶고, 외부에서 직접 접근하지 못하도록 숨기는 것

```java
public class Account {
    private int balance; // 외부에서 직접 접근 불가

    public void deposit(int amount) {
        if(amount > 0) {
            balance += amount;
        }
    }

    public int getBalance() {
        return balance;
    }
}

```


**상속**은 기존 클래스의 속성과 메소드를 새로운 클래스가 물려받는 것. 동물 클래스가 있으면 개 클래스가 동물의 특성을 상속받아서 코드 재사용성을 높힐수 있다.

```java
public class Animal {
    protected String name;
    public void eat() { System.out.println("먹는다"); }
}

public class Dog extends Animal {
    public void bark() { System.out.println("멍멍"); }
}

```


**다형성**은 같은 메소드 호출이지만 객체에 따라 다르게 동작하는 것.

```java
public interface Shape {
    void draw();
}

public class Circle implements Shape {
    public void draw() { System.out.println("원을 그린다"); }
}

public class Rectangle implements Shape {
    public void draw() { System.out.println("사각형을 그린다"); }
}

// 사용
Shape[] shapes = {new Circle(), new Rectangle()};
for(Shape shape : shapes) {
    shape.draw(); // 각각 다른 방식으로 동작
}

```

**추상화**는 복잡한 구현 내용은 숨기고 필요한 기능만 노출하는 것입니다. 자동차를 운전할 때 엔진 내부 구조를 몰라도 시동키만 돌리면 되는 것 처럼


```java
public abstract class Vehicle {
    protected String brand;

    public abstract void start(); // 추상 메소드

    public void stop() { // 구체 메소드
        System.out.println("정지");
    }
}

public class Car extends Vehicle {
    public void start() {
        System.out.println("시동을 건다");
    }
}

```


