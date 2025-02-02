---
date: 2024 년 11 월 26 일 10 시 11 분
tags:
  - Dev
author: Joung Dong Hee
share: true
---

# XML 로 Bean 을 생성 하는 방법

Spring에서는 다양한 방법으로 [Spring Bean](Spring%20Bean.md)을 생성하고 주입할 수 있습니다. 그중 **XML을 이용한 Bean 설정**은 과거에 가장 보편적으로 사용되던 방식이었습니다.

현재는 **Java 기반 설정(Annotation 기반 설정 포함)** 방식이 더 많이 사용되지만, 레거시 프로젝트에서는 여전히 XML 방식을 활용하는 경우가 많습니다. 따라서 학습 차원에서 XML 기반 Bean 설정 방법을 정리해 보겠습니다.


## XML을 이용한 Bean 등록

XML을 이용하여 스프링 빈을 등록할 때는 `<bean>` 태그를 사용하며, `id` 속성은 빈의 이름을, `class` 속성은 해당 빈의 클래스 정보를 나타냅니다. **의존성이 필요한 경우** `<constructor-arg>` 또는 `<property>` 태그를 이용하여 의존성 주입을 할 수 있습니다.


```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
       http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <!-- MemberService 빈 정의 (생성자 주입) -->
    <bean id="memberService" class="hello.core.member.MemberServiceImpl">  
        <constructor-arg name="memberRepository" ref="memberRepository"/>  
    </bean>  
  
    <!-- MemberRepository 빈 정의 -->
    <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>  
  
    <!-- OrderService 빈 정의 (생성자 주입) -->
    <bean id="orderService" class="hello.core.order.OrderServiceImpl">  
        <constructor-arg name="memberRepository" ref="memberRepository"/>  
        <constructor-arg name="discountPolicy" ref="discountPolicy"/>  
    </bean>  
  
    <!-- DiscountPolicy 빈 정의 -->
    <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy"/>  
  
</beans>

```


## XML 기반 빈을 가져오는 방법

등록된 XML 설정을 활용하여 `GenericXmlApplicationContext`를 통해 `ApplicationContext`를 생성하고, `getBean` 메서드를 사용하여 빈을 조회할 수 있습니다.

```java
ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");
MemberService memberService = ac.getBean("memberService", MemberService.class);
```


## **XML 설정 방식의 단점**

- XML 파일이 많아지면 관리가 어려워짐.
- 타입 안전성이 떨어짐 (컴파일 시점이 아닌 런타임 시점에서 오류 발견).
	- 즉 사전에 IDE 와 같은 프로그램의 도움 없이 프로그램 동작 시 에 오류를 찾을수 있음
- Spring Boot에서는 기본적으로 XML 기반 설정을 사용하지 않음.

## 결론

XML 방식의 Bean 설정은 **과거에는 표준적이었지만**, 현재는 Java 기반 설정과 `@ComponentScan`을 활용한 방식이 더 많이 사용됩니다.

하지만 **레거시 프로젝트 유지보수 및 Spring의 핵심 개념을 이해하기 위해 XML 설정 방법을 익혀두는 것은 유용**합니다. 🚀