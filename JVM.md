# JVM 메모리 관리
## 참고 및 공부 글
- [JVM, 가상머신](https://asfirstalways.tistory.com/158)
- [가비지컬렉션](https://asfirstalways.tistory.com/159)



## 개요
JVM(Java Virtual Machine)은 Java 프로그램이 실행될 때 메모리를 효율적으로 관리한다. 개발자가 직접 메모리를 할당하고 해제하지 않아도 되는 이유가 바로 JVM의 자동 메모리 관리 때문이다.

## JVM 메모리 구조

JVM의 메모리는 크게 **메서드 영역**, **힙 영역**, **스택 영역**으로 나뉜다.

```
┌─────────────────────────────────────┐
│            Method Area              │  ← 클래스 정보, static 변수
├─────────────────────────────────────┤
│               Heap                  │  ← 객체들이 저장되는 곳
│  ┌─────────────┬─────────────────┐  │
│  │ Young Gen   │   Old Gen       │  │
│  └─────────────┴─────────────────┘  │
├─────────────────────────────────────┤
│             Stack               │  ← 메서드 호출, 지역변수
└─────────────────────────────────────┘
```

### 1. 메서드 영역 (Method Area)
**용도**: 클래스 수준의 정보를 저장

**저장되는 것들**:
- 클래스의 메타데이터 (클래스명, 부모클래스명, 메서드 정보 등)
- static 변수들
- 상수 풀 (Constant Pool)
- 메서드의 바이트코드

```java
public class Example {
    static int staticVar = 10;  // 메서드 영역에 저장
    final String CONSTANT = "Hello";  // 메서드 영역에 저장
}
```

**특징**:
- 모든 스레드가 공유
- JVM 시작 시 생성되어 프로그램 종료까지 유지

### 2. 힙 영역 (Heap)
**용도**: 객체와 인스턴스 변수들을 저장

힙은 다시 **Young Generation**과 **Old Generation**으로 나뉜다.

#### Young Generation
새로 생성된 객체들이 저장되는 곳
- **Eden**: 새 객체가 처음 생성되는 곳
- **Survivor 0, 1**: GC에서 살아남은 객체들이 임시로 머무는 곳

#### Old Generation  
Young Generation에서 오래 살아남은 객체들이 이동하는 곳

```java
public class HeapExample {
    public static void main(String[] args) {
        Person p1 = new Person("김철수");  // 힙의 Eden 영역에 저장
        Person p2 = new Person("박영희");  // 힙의 Eden 영역에 저장
    }
}

class Person {
    String name;  // 힙에 저장되는 인스턴스 변수
    
    Person(String name) {
        this.name = name;
    }
}
```

**특징**:
- 모든 스레드가 공유
- 가비지 컬렉션의 대상

### 3. 스택 영역 (Stack)
**용도**: 메서드 호출과 관련된 데이터 저장

**저장되는 것들**:
- 지역 변수
- 메서드 매개변수  
- 메서드 호출 정보
- 연산 중 임시 데이터

```java
public void stackExample() {
    int localVar = 20;        // 스택에 저장
    String localString = "Hi"; // 참조값은 스택, 실제 객체는 힙
    
    methodCall(localVar);     // 메서드 호출 정보가 스택에 쌓임
}
```

**특징**:
- 각 스레드마다 별도의 스택 존재
- 메서드 호출 시 스택 프레임이 생성되고, 메서드 종료 시 제거
- LIFO(Last In, First Out) 구조

## 가비지 컬렉션 (Garbage Collection)

### 가비지 컬렉션이란?
더 이상 참조되지 않는 객체들을 자동으로 메모리에서 해제하는 기능이다.

### 언제 가비지가 될까?
```java
public void gcExample() {
    Person p1 = new Person("김철수");
    Person p2 = new Person("박영희");
    
    p1 = null;  // 김철수 객체는 이제 가비지가 됨
    p2 = p1;    // 박영희 객체도 가비지가 됨
    
    // GC가 실행되면 두 객체 모두 메모리에서 제거됨
}
```

## 핵심 정리

JVM의 메모리 관리는 다음과 같은 원칙으로 동작한다:

1. **자동 관리**: 개발자가 직접 메모리를 할당/해제하지 않음
2. **영역별 분리**: 메서드, 힙, 스택 영역으로 나누어 효율적 관리
3. **가비지 컬렉션**: 사용하지 않는 객체를 자동으로 정리
4. **세대별 GC**: 객체의 수명에 따라 다른 전략으로 관리

개발자는 메모리 누수를 방지하고, 필요시 JVM 옵션을 조정하여 성능을 최적화할 수 있다.
