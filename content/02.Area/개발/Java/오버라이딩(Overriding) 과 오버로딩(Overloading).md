---
date: 2024 년 10 월 05 일 22 시 10 분
tags:
  - Dev
  - Java
  - Overriding
  - Overloading
share: true
---

자바에서는 간단하지만 자주 혼동되는 두 가지 개념이 있습니다. 바로 **오버로딩(Overloading)** 과 **오버라이딩(Overriding)** 입니다.

# 오버로딩(Overloading)

오버로딩은 **메서드 이름은 동일하지만, 파라미터의 수나 타입이 다른** 경우를 의미하며, 같은 클래스 내에서 여러 번 정의할 수 있습니다. 이는 메서드를 다양한 형태로 사용할 수 있게 해 주며, 하나의 기능을 여러 방식으로 처리할 수 있게 해줍니다.

## 특징

1. **메서드의 이름이 같아야** 합니다.
2. **반환 타입(Return type)** 은 같을 수도, 다를 수도 있습니다.
3. **파라미터의 개수가 달라야** 합니다.
4. **파라미터의 개수가 같다면 데이터 타입이 달라야** 합니다.


## Example Code

다음과 같이 `BoardClass` 내부에 여러 개의 `select` 메서드를 정의할 수 있습니다. 이처럼 같은 메서드 이름을 사용하지만 인자값에 따라 다르게 동작하도록 메서드를 정의하는 것을 **오버로딩(Overloading)** 이라고 합니다.

```java
public class BoardClass {  
    // 파라미터가 없는 select 메서드  
    public void select() {  
        System.out.println("게시판 전체 조회");  
    }  
  
    // 파라미터가 한 개인 select 메서드  
    public void select(int count) {  
        System.out.println("게시판 " + count + " 개 조회");  
    }  
  
    // 파라미터가 두 개인 select 메서드  
    public void select(int count, int total) {  
        System.out.println("게시판 " + count + " 개 조회, 게시판 총 갯수: " + total);  
    }  
}

```


# 오버라이딩(Overriding) 

오버라이딩은 오버로딩과는 다르게, **상속 관계에 있는 클래스** 간에 **부모 클래스의 메서드를 자식 클래스에서 재정의**하는 것을 의미합니다. 여기서 중요한 키워드는 **상속**과 **재정의**입니다. 부모 클래스로부터 상속받은 메서드를 자식 클래스에서 동일한 메서드 시그니처로 다시 정의해 원하는 대로 동작하게 할 수 있습니다.

## 특징

1. **오버라이드하고자 하는 메서드가 상위 클래스에 존재해야** 합니다.
2. **메서드의 이름이 같아야** 합니다.
3. **파라미터의 개수와 자료형이 같아야** 합니다.
4. **반환 타입(Return type)** 도 같아야 합니다.

## Example Code

다음은 `Car` 클래스에서 `move` 메서드를 정의한 예시입니다.

### Car class
```java
public class Car {  
    public String category;  
    
    public void move(Car car) {  
        System.out.println(car.category + " 자동차가 움직입니다.");  
    }  
}

```

### GasCar class

그다음 `GasCar`라는 클래스를 정의하여, `Car` 클래스로부터 상속받습니다. 그리고 상속받은 `move` 메서드를 재정의합니다. 여기서 **@Override** 어노테이션을 사용하여 해당 메서드가 오버라이딩된 것임을 명확히 표시할 수 있습니다.

```java
public class GasCar extends Car {  
    int total;  
  
    @Override  
    public void move(Car car) {  
        System.out.println(car.category + " 자동차가 움직입니다.");  
        // 추가
        System.out.println("총 기름량: " + total);  
    }  
}

```



#### @Override 어노테이션

`@Override` 어노테이션은 오버라이딩을 명시적으로 나타내며, 코드의 가독성을 높이고 실수를 방지할 수 있습니다. 이를 사용하면 컴파일러가 부모 클래스의 메서드를 정확히 재정의하고 있는지 확인하므로, 잘못된 시그니처로 인한 오류를 방지할 수 있습니다.

## 오버로딩과 오버라이딩의 차이점 정리

1. **오버로딩(Overloading)** 은 **같은 클래스 내**에서 **메서드 이름은 같지만, 파라미터가 다르게 정의**되는 것을 말합니다. 즉, 여러 메서드가 같은 이름을 가질 수 있으며 메서드의 매개변수나 타입에 따라 다르게 동작합니다.
    - **같은 클래스** 내에서 적용됨.
    - **파라미터의 개수나 타입이 달라야** 함.
    - **리턴 타입은 같거나 다를 수 있음.**
2. **오버라이딩(Overriding)** 은 **상속 관계**에서 **부모 클래스의 메서드를 자식 클래스에서 재정의**하는 것을 의미합니다. 이는 다형성을 실현하는 중요한 방법 중 하나입니다.
    - **상속 관계**에서 적용됨.
    - **메서드의 시그니처(이름, 파라미터 타입, 개수, 리턴 타입)가 동일**해야 함.
    - 부모 클래스의 동작을 **자식 클래스에서 다른 방식으로 재정의**할 수 있음.
