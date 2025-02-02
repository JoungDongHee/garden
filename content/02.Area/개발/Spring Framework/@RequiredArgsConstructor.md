---
date: 2025 년 02 월 02 일 23 시 02 분
tags:
  - Dev
author: Joung Dong Hee
share: true
---

# @RequiredArgsConstructor

`lombok` 라이브러리에서 제공하는 어노테이션 중 `@RequiredArgsConstructor`가 존재한다.  
이 어노테이션은 특정 조건을 만족하는 필드를 매개변수로 하는 **생성자를 자동으로 생성해주는 어노테이션**이다.


## @RequiredArgsConstructor 사용전

[ Spring 생성자 의존성 주입](Spring%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85%20DI(Dependency%20Injection).md#^8d0d3f) 방식에서는 다음과 같이 `@Autowired`와 생성자를 사용하여 의존성을 주입해야 한다.

```java
@Component  
public class MemberServiceImpl implements MemberService {  
    private final MemberRepository memberRepository;  

    @Autowired // ac.getBean(MemberRepository.class)  
    public MemberServiceImpl(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    } 
}

```

그러나 **생성자가 단 하나만 존재하는 경우, Spring에서는 `@Autowired`를 생략해도 자동으로 의존성 주입이 가능**하다.  
따라서 다음과 같이 `@Autowired` 없이 생성자만 정의해도 동일하게 동작한다.

```java
@Component  
public class MemberServiceImpl implements MemberService {  
    private final MemberRepository memberRepository;  

    public MemberServiceImpl(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    }  
}

```


## @RequiredArgsConstructor 사용후

`@RequiredArgsConstructor`를 사용하면 위의 생성자도 생략할 수 있다.  
즉, Lombok이 **`final`이 붙은 필드와 `@NonNull`이 붙은 필드를 매개변수로 받는 생성자**를 자동으로 생성해준다.


```java
@Component  
@RequiredArgsConstructor  
public class MemberServiceImpl implements MemberService {  
    private final MemberRepository memberRepository;  
}

```

컴파일 이후 실제 생성된 클래스는 다음과 같은 형태가 된다.

```java
@Component  
public class MemberServiceImpl implements MemberService {  
    private final MemberRepository memberRepository;  

    public MemberServiceImpl(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    }  
}

```


## @RequiredArgsConstructor의 동작 원리

- **`final`이 붙은 필드**를 매개변수로 받는 생성자를 자동으로 생성한다.
- **`@NonNull`이 붙은 필드**도 생성자의 매개변수로 포함되며, `null` 방지 코드를 추가한다.
- `final`이나 `@NonNull`이 없는 필드는 생성자에 포함되지 않는다.

### 예제 1: `final` 필드만 포함되는 경우

```java
@RequiredArgsConstructor
public class Example {
    private final String name;
    private int age; // final이 아님 -> 생성자에 포함되지 않음
}
```

컴파일 후:

```java
public class Example {
    private final String name;
    private int age;

    public Example(String name) { // `age`는 포함되지 않음
        this.name = name;
    }
}

```


### 예제 2: `@NonNull`이 붙은 필드 포함


```java
@RequiredArgsConstructor
public class Example {
    private final String name;
    @NonNull
    private String email;
}

```

컴파일 후:

```java
public class Example {
    private final String name;
    private String email;

    public Example(String name, String email) {
        this.name = name;
        if (email == null) {
            throw new NullPointerException("email is marked non-null but is null");
        }
        this.email = email;
    }
}

```


# 정리

- `@RequiredArgsConstructor`는 **`final` 필드와 `@NonNull` 필드를 매개변수로 받는 생성자**를 자동으로 생성한다.
- Spring 의존성 주입에서 **생성자가 하나일 경우 `@Autowired`를 생략할 수 있으므로, `@RequiredArgsConstructor`를 활용하면 코드가 더 간결해진다.**