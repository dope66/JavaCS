# 캡슐화 (Encapsulation)

## 1. 정의
- 데이터와 메서드를 하나로 묶고, 외부에서 직접 접근하지 못하게 막는 개념
- 객체의 내부 상태를 보호하여 무결성을 유지

## 2. 핵심 키워드
- 접근 제어자 (private, protected, public)
- 정보 은닉 (Information Hiding)

## 3. 예시 코드

```java
public class Account {
    private int balance;   // 외부에서 직접 접근 불가

    public void deposit(int amount) {
        if (amount > 0) balance += amount;
    }

    public int getBalance() {
        return balance;
    }
}
```

