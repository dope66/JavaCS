# 오버로딩과 오버라이딩 (Overloading vs Overriding)

##  개념 정리

###  오버로딩 (Overloading)
같은 클래스 내에서 **같은 이름의 메소드**를 **매개변수만 다르게** 해서 여러 개 만드는 것

- **핵심**: "새로 만들기" - 기능은 비슷하지만 입력이 다른 여러 버전 생성
- **결정 시점**: 컴파일 시점 (정적 바인딩)

###  오버라이딩 (Overriding)  
상속 관계에서 **부모 클래스의 메소드**를 **자식 클래스에서 재정의**하는 것

- **핵심**: "다시 만들기" - 상속받은 메소드의 동작을 새롭게 정의
- **결정 시점**: 런타임 시점 (동적 바인딩)

---

##  상세 비교

| 구분 | 오버로딩 (Overloading) | 오버라이딩 (Overriding) |
|------|------------------------|-------------------------|
| **정의** | 같은 이름의 메소드를 매개변수만 다르게 여러 개 정의 | 상위 클래스의 메소드를 하위 클래스에서 재정의 |
| **발생 위치** | 같은 클래스 내부 | 상속 관계의 클래스 간 |
| **발생 시점** | 컴파일 타임 (정적) | 런타임 (동적) |
| **메소드명** | 동일 | 동일 |
| **매개변수** | 다름 (개수, 타입, 순서) | 동일 |
| **리턴 타입** | 상관없음 | 동일 (또는 하위 타입) |
| **접근 제어자** | 상관없음 | 같거나 더 넓은 범위 |

---

##  코드 예시

###  오버로딩 예시
```java
public class Calculator {
    // 정수 2개 더하기
    public int add(int a, int b) {
        return a + b;
    }
    
    // 실수 2개 더하기
    public double add(double a, double b) {
        return a + b;
    }
    
    // 정수 3개 더하기
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // 문자열 연결
    public String add(String a, String b) {
        return a + b;
    }
}

// 사용 예시
Calculator calc = new Calculator();
System.out.println(calc.add(1, 2));           // 3 (int)
System.out.println(calc.add(1.5, 2.3));       // 3.8 (double)
System.out.println(calc.add(1, 2, 3));        // 6 (int)
System.out.println(calc.add("Hello", "World")); // HelloWorld (String)
```

### 오버라이딩 예시
```java
// 부모 클래스
public class Animal {
    public void makeSound() {
        System.out.println("동물이 소리를 냅니다");
    }
    
    public void sleep() {
        System.out.println("동물이 잠을 잡니다");
    }
}

// 자식 클래스 1
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("야옹");
    }
}

// 자식 클래스 2
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("멍멍");
    }
}

// 사용 예시
Animal cat = new Cat();
Animal dog = new Dog();

cat.makeSound(); // "야옹" - Cat의 오버라이딩된 메소드 실행
dog.makeSound(); // "멍멍" - Dog의 오버라이딩된 메소드 실행
cat.sleep();     // "동물이 잠을 잡니다" - 부모의 메소드 실행
```

---

##  핵심 포인트

### 오버로딩의 특징
-  **다양한 입력 처리**: 같은 기능을 다양한 매개변수로 제공
-  **편의성 증대**: 사용자가 상황에 맞는 메소드 선택 가능
-  **컴파일 시점 결정**: 성능상 이점

### 오버라이딩의 특징
-  **다형성 구현**: 같은 메소드 호출로 객체별 다른 동작
-  **상속의 활용**: 부모의 기능을 자식이 특화해서 구현
-  **런타임 결정**: 실제 객체 타입에 따라 메소드 선택

---

##  기억하기 쉬운 방법

> **오버로딩** = "새로 만들기" 
> - 같은 이름으로 **새로운 버전**들을 만드는 것
> 
> **오버라이딩** = "다시 만들기" 
> - 물려받은 것을 **다시 정의**하는 것
