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

# Spring 의존성 주입을 하기 위한 방법

[Spring Framework](Spring%20Framework.md) 에서는 다양한 방법으로 `의존성 주입` 이 가능하다. 


## 1. 필드 주입 (Field Injection)

필드 자체에 `@Autowired` 어노테이션을 붙이면 Spring에서 생성한 Bean 객체를 자동으로 주입받을 수 있다.

### **장점**

- 코드가 간결하며, 따로 생성자나 Setter 메서드를 만들 필요가 없다.

### **단점**

- **단위 테스트가 어렵다**: 필드에 직접 주입되므로, 테스트 시 Mock 객체를 설정하기 어렵다.
- **불변성이 보장되지 않는다**: 객체가 생성된 후에도 필드 값을 변경할 수 있다.
- **Spring 컨테이너 없이 사용할 수 없다**: DI 컨테이너 없이 순수 Java 객체로 활용하기 어렵다.
### **예제 코드**

``` java
@Component
class A {
    @Autowired
    private B b; // B의 구현체를 자동으로 주입
}
```

> 💡 **권장되지 않는 방식**: 테스트와 유지보수를 어렵게 만들기 때문에 지양하는 것이 좋다.

---

## 2. 수정자 주입 (Setter Injection)

`setter` 메서드를 통해 의존성을 주입하는 방식이다. 이 방식은 선택적 의존성을 처리할 때 유용할 수 있다.

### **장점**

- **선택적 의존성 주입 가능**: 특정 의존성이 필요하지 않을 때 유연하게 설정할 수 있다.
- **Spring 컨테이너 없이도 객체 생성 가능**: 필수적인 의존성이 없어도 객체를 만들 수 있다.

### **단점**

- **의존성이 불변하지 않음**: 생성된 객체의 의존성이 변경될 수 있다.
- `NullPointerException` **발생 가능성**: 의존성이 나중에 주입되므로, 객체가 사용되기 전에 주입되지 않으면 `NullPointerException`이 발생할 수 있다.

### **예제 코드**

```java
@Component
public class MemberService {
    private MemberRepository memberRepository;
  
    @Autowired  
    public void setMemberRepository(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    }
}
```

> 💡 **선택적 의존성이 필요할 때만 사용**: 하지만 생성자 주입이 가능한 경우 권장되지 않는다.

---

## 3. 생성자 주입 (Constructor Injection) - **권장** ✅

^8d0d3f

생성자를 사용하여 의존성을 주입하는 방식으로 가장 많이 권장되는 방법이다.

### **장점**

- **불변성 보장**: `final` 키워드를 활용하여 객체가 생성된 이후 변경을 방지할 수 있다.
- **의존성이 항상 주입됨**: 생성자에서 주입되므로 `null` 상태가 될 가능성이 없다.
- **테스트 용이**: 의존성을 명확하게 주입받기 때문에 테스트 시 별도의 설정이 필요 없다.
- **Spring 컨테이너 없이도 활용 가능**: 순수 Java 코드에서도 객체를 쉽게 생성할 수 있다.

### **예제 코드**

``` java
@Component
public class MemberService {
    private final MemberRepository memberRepository;
  
    @Autowired // Spring 4.3 이상에서는 생략 가능
    public MemberService(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    }
}
```

> 💡 **가장 권장되는 방식**: 불변성과 명확한 의존성을 보장할 수 있기 때문에 Spring에서 가장 많이 사용된다.

## **정리**

| 방식         | 장점                 | 단점                   | 사용 추천    |
| ---------- | ------------------ | -------------------- | -------- |
| **필드 주입**  | 코드가 간결함            | 테스트 어려움, 불변성 없음      | 🚫 지양    |
| **수정자 주입** | 선택적 의존성 처리 가능      | 의존성 변경 가능, `null` 위험 | ⚠ 선택적 사용 |
| **생성자 주입** | 불변성 보장, 명확한 의존성 관리 | 코드가 약간 길어질 수 있음      | ✅ 권장     |

> **결론**: Spring에서 의존성 주입을 할 때 **생성자 주입**을 기본적으로 사용하고, 선택적 의존성이 필요한 경우에만 수정자 주입을 고려하는 것이 좋다.