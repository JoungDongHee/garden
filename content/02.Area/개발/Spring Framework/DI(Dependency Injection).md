---
date: 2024 년 11 월 15 일 00 시 11 분
tags:
  - Dev
  - SpringFramwork
  - DI
  - ioC
author: Joung Dong Hee
share: true
---

# DI(Dependency Injection)

Dependency Injection 은 **코드 간의 의존성을 외부에서 관리하고 주입하여 결합도를 낮추는 설계 패턴**입니다.

다음과 같이 두개의 클래스가 있다. 이때 클래스 A 에는 B 의 클래스를 사용하여 인스턴스를 생성하고 있다. 

이 처럼 A 클래스에서는 B 를 의존하여 사용한다고 하여 A 는 B 에 의존하고 있다고 한다.

```java
class B {
    // B 클래스 로직
}

```

```java
class A {
    B b = new B(); // A 클래스가 B 클래스의 구현에 직접 의존
}
```

이 코드에서는 `A` 클래스가 `B` 클래스의 인스턴스를 직접 생성하고 있기 때문에 **강한 결합**이 발생합니다.  
즉, `B`의 구현체가 변경되면 `A` 클래스도 수정이 필요하므로 유연성이 떨어집니다.

## 의존성 주입의 기본 개념

의존성 주입을 사용하면 `B` 클래스의 인스턴스를 `A` 클래스 내부에서 생성하지 않고, 외부에서 전달(주입)받습니다.  
이를 통해 `A` 클래스는 `B` 클래스의 구현 세부사항에 의존하지 않고, **인터페이스를 통해 느슨한 결합**을 유지할 수 있습니다.

## 의존성 주입을 사용한 예

아래의 코드는 의존성 주입을 사용한 예시이다. 

`B` 인터페이스를 사용하여 `BImpl` 구현체를 만들었습니다. 

**`A` 클래스는 `B` 인터페이스에 의존하며, 구현체를 직접 생성하거나 관리하지 않고 외부에서 주입받는 방식으로 결합도를 낮췄습니다.**  
이를 통해 `A` 클래스는 `B` 인터페이스를 구현하는 다른 어떤 객체라도 사용할 수 있습니다.

DI를 통해 `A` 클래스는 `B` 구현체의 생성과 관리를 외부에 위임하므로, `B`를 구현한 다른 객체로 교체하기 쉽습니다.

```java
interface B {
    void doSomething();
}

class BImpl implements B {
    @Override
    public void doSomething() {
        System.out.println("BImpl is working");
    }
}

class A {
    private final B b;

    // 생성자를 통한 의존성 주입
    public A(B b) {
        this.b = b;
    }

    public void execute() {
        b.doSomething();
    }
}

```




## 의존성 주입을 하기 위한 방법

[Spring Framework](Spring%20Framework.md) 에서는 다양한 방법으로 의존성 주입이 가능하다. 대표적엔 예로 어노테이션을 사용한 `@Autowired` 를 사용한 방법이 존재하고 그다음은 `생성자를 사용한 방법`이다. 

### 1. 필드 주입

```java
@Component
class A {
    @Autowired
    private B b; // B의 구현체를 자동으로 주입
}

```

- **장점**: 간단하게 작성 가능.
- **단점**: 테스트 시 Mock 객체 주입이 어렵고, DI 컨테이너 없이는 동작하지 않음. 권장되지 않는 방식.

### 2. 생성자 주입(권장)

```java
@Component
class A {
    private final B b;

    @Autowired // Spring Boot에서는 생략 가능
    public A(B b) {
        this.b = b;
    }
}

```

- **장점**: 불변성과 테스트 용이성을 보장. 권장되는 방식.

## 의존성 주입의 장점

1. **결합도 감소**  
    클래스 간의 강한 결합을 방지하여 코드 변경 및 확장이 쉬워집니다.  
    예: 다른 구현체로 교체하거나 테스트 시 Mock 객체를 주입하는 것이 용이합니다.
    
2. **테스트 용이성**  
    외부에서 의존성을 주입받기 때문에, 실제 구현체 대신 Mock 객체를 주입하여 단위 테스트를 쉽게 수행할 수 있습니다.
    
3. **재사용성과 유연성 증가**  
    의존성 주입으로 코드는 특정 구현에 얽매이지 않고, 다양한 상황에서 재사용할 수 있습니다.
