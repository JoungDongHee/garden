---
date: 2024 년 11 월 14 일 23 시 11 분
tags:
  - Dev
  - Bean
author: Joung Dong Hee
share: true
---

# Spring Bean

Spring Bean 은  [IoC컨테이너](IoC(Inversion%20of%20Control).md#^0e3b30) 가 관리하는 객체를 의미하며, **객체의 생성, 의존성 관리, 생명주기 등을 관리**한다.  
스프링에서 Bean을 등록하는 방법은 다음과 같다.

- `@Component` 및 그 하위 어노테이션(`@Service`, `@Repository`, `@Controller`) 사용
- `@Configuration` 클래스에서 `@Bean` 메서드 사용

아래는 `@Configuration`을 활용한 예제이다.

```java
@Configuration  
public class AppConfig {  

    @Bean  
    public MemberService memberService(){  
        return new MemberServiceImpl(memberRepository());  
    }  
  
    @Bean  
    public MemoryMemberRepository memberRepository() {  
        return new MemoryMemberRepository();  
    }  
  
    @Bean  
    public OrderService orderService(){  
        return new OrderServiceImpl(memberRepository(), discountPolicy());  
    }  
  
    @Bean  
    public DiscountPolicy discountPolicy() {  
        return new RateDisCountPolicy();  
    }  
  
}
```

## `@Configuration`과 싱글톤 패턴

스프링에서는 기본적으로 [싱글톤 패턴(Singleton Pattern)](%EC%8B%B1%EA%B8%80%ED%86%A4%20%ED%8C%A8%ED%84%B4(Singleton%20Pattern).md) 을 적용하여 **Bean을 한 번만 생성**하고, 동일한 객체를 재사용한다.  
이를 위해 `@Configuration`을 클래스에 선언하면, 내부적으로 **CGLIB 프록시**가 생성되어 `@Bean` 메서드가 여러 번 호출되더라도 **동일한 인스턴스**를 반환하도록 보장한다.

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

MemberService memberService1 = context.getBean(MemberService.class);
MemberService memberService2 = context.getBean(MemberService.class);

System.out.println(memberService1 == memberService2); // true (동일한 객체)

```

### `@Configuration`을 생략하면?

만약 `@Configuration`을 생략하고 `@Bean`만 사용하면 **싱글톤이 깨질 가능성이 있다.**  
이 경우 `@Bean` 메서드가 호출될 때마다 새로운 객체가 생성될 수 있으므로 주의해야 한다.


## `@ComponentScan` 을 사용한 자동 빈 등록

이전 예제에서는 `@Configuration`을 사용하여 `@Bean`을 직접 등록했지만, 스프링에서는 `@ComponentScan`을 사용하여 **자동으로 Bean을 등록**할 수도 있습니다.



대규모 프로젝트에서는 모든 Bean을 수동으로 등록하는 것이 어렵고 번거로울 수 있습니다. 이를 해결하기 위해 **`@ComponentScan`** 을 사용하면 특정 패키지와 그 하위 패키지에 있는`@Service` ,`@Controller` ,`@Component`, `@Service`, `@Repository` 등의 어노테이션이 붙은 클래스를 자동으로 스캔하여 Bean으로 등록합니다.  
또한, **`@Autowired`를 활용한 생성자 주입** 을 사용하면 **불변성을 보장**하고 **테스트가 용이해지는 장점**이 있습니다.


```java
@Repository // 자동으로 Spring Bean 등록
public class MemberRepository {
    public String getData() {
        return "Member Data";
    }
}

```

`MemberService` 클래스는 `MemberRepository`를 의존하고 있습니다.

```java
@Service // 자동으로 Spring Bean 등록
public class MemberService {
    private final MemberRepository memberRepository;

    // 생성자 주입 방식으로 의존성 주입
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    public String getMemberInfo() {
        return memberRepository.getData();
    }
}

```


`@ComponentScan`을 사용하면 **지정한 패키지와 그 하위 패키지에 있는 모든 `@Component`가 붙은 클래스를 자동으로 스캔하여 Bean으로 등록**합니다.

```java
@Configuration
@ComponentScan(basePackages = "com.example") // com.example 패키지 및 하위 패키지를 자동 스캔
public class AppConfig {
}

```

> **💡 참고:**  
> `@ComponentScan`을 선언하지 않더라도 `@SpringBootApplication` 내부에서 이미 기본적으로 설정됩니다.  
> 다만, 특정 패키지만 스캔하고 싶을 경우 `basePackages` 속성을 설정하면 됩니다.


사용할때는 수동 으로 사용할때와 동일하다.

```java
public class MainApp {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(com.example.config.AppConfig.class);

        // 자동으로 등록된 Bean 가져오기
        MemberService memberService = context.getBean(MemberService.class);
        System.out.println(memberService.getMemberInfo()); // "Member Data" 출력
    }
}

```

## `Spring Bean` 조회

Spring IoC 컨테이너 내부에서는 `Bean Name`과 `Class` 형태로 객체가 등록된다.

|빈 이름 (Bean Name)|클래스 (Class)|
|---|---|
|userController|com.example.UserController|
|userService|com.example.UserServiceImpl|
|userMapper|com.example.UserMapper|
|emailUtil|com.example.EmailUtil|
|qrUtil|com.example.QrUtil|
|thymeleafConfig|com.example.ThymeleafConfig|
|dataSource|com.zaxxer.hikari.HikariDataSource|


# 주요 메서드

### `getBean(String name)`

등록된 Bean을 **이름으로 조회**할 때 사용한다.

```java
MemberService memberService = (MemberService) context.getBean("memberService");  

```


### `getBean(String name, Class<T> requiredType)`

등록된 Bean을 **이름과 클래스 타입으로 조회**할 때 사용한다.

```java
MemberService memberService = context.getBean("memberService", MemberService.class);  
Member member = memberService.findMember(1L);
```


### `getBean(Class<T> requiredType)`

등록된 Bean을 **클래스 타입만으로 조회**할 때 사용한다.  
하지만, **동일한 클래스의 Bean이 여러 개 있을 경우 `NoUniqueBeanDefinitionException` 예외가 발생**한다.

```java
MemberService memberService = context.getBean(MemberService.class);  
```

### `getBean(Object.class)`

`Object.class`를 매개변수로 전달하면 **모든 Bean이 조회**된다.  
이유는 `Object`가 **Java의 최상위 클래스**이기 때문이다.

```java
Map<String, Object> beans = context.getBeansOfType(Object.class);
```


